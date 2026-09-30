+++
title = "Ngày 02 - 30/09/2026 (Office)"
weight = 2
+++

# BÁO CÁO THỰC HÀNH — NGÀY 02

## Domain D & E – Core Inventory Ledger, Triggers, Optimistic Locking & Query APIs

### 1. Chi tiết thực hiện & Kết quả đạt được

* **Thiết lập & Đồng bộ Database Engine (PostgreSQL & Prisma Migration)**:
  - Khởi tạo toàn bộ 5 domain dữ liệu từ `schema.prisma` sang cơ sở dữ liệu PostgreSQL cục bộ thông qua migration `20260930021017_init_all_domains`.
  - Thực thi các ràng buộc cấp cơ sở dữ liệu không hỗ trợ thuần bằng Prisma schema: 3 CHECK constraints (`chk_qty_on_hand_nonneg`, `chk_qty_reserved_nonneg`, `chk_qty_reserved_le_onhand`), cột ảo tính toán tự động lưu trữ `qty_available = qty_on_hand - qty_reserved` (`GENERATED ALWAYS AS ... STORED`), và partial unique index `user_roles_null_wh_uk` cho quyền toàn hệ thống khi `warehouse_id IS NULL`.
  - Bổ sung migration `20260930022500_enforce_stock_ledger_append_only`: Cài đặt trigger `trg_prevent_stock_ledger_mutation` và table-level statement trigger `trg_prevent_stock_ledger_truncate` trên bảng `stock_ledger`, ngăn chặn triệt để mọi hành vi `UPDATE`, `DELETE`, và `TRUNCATE` ở cấp database engine.
  - Seed dữ liệu chuẩn: 7 roles nghiệp vụ chuẩn WMS (`ADMIN`, `MANAGER`, `SUPERVISOR`, `RECEIVER`, `PICKER`, `INSPECTOR`, `VIEWER`), tài khoản admin hệ thống, 10 loại biến động kho (`movement_types` kèm dấu `sign` +1/-1) và 6 loại chứng từ tham chiếu (`reference_types`).

* **Xây dựng `InventoryLedgerService` (Lõi quản trị biến động tồn kho)**:
  - **`recordMovement(input)`**:
    - Thực thi toàn bộ chuỗi nghiệp vụ trong một `prisma.$transaction`.
    - Kiểm tra tính hợp lệ của độ lớn số lượng `quantity > 0` (`Prisma.Decimal`), chặn đứng số âm và số 0 bằng `BadRequestException`.
    - Tra cứu `movement_types` theo mã, tính toán dấu toán học `qtyChange = sign * quantity` (không nhận trực tiếp `qtyChange` có dấu từ caller).
    - Xác thực tính toàn vẹn kho: so khớp `warehouseId` gửi lên với `warehouse_id` thật của `locationId`.
    - Tự động khởi tạo dòng `inventory` mới với `version = 0` khi SKU lần đầu phát sinh tại vị trí lưu trữ.
    - Áp dụng cơ chế **Optimistic Locking** (`WHERE id = :id AND version = :old_version`, đồng thời tăng `version = version + 1`). Bắt chính xác trường hợp xung đột phiên bản và ném `ConflictException` (409) cho phép caller retry.
    - Ghi nhận bút toán vào `stock_ledger` theo nguyên tắc bất biến (append-only), lưu trữ đầy đủ `qtyBefore`, `qtyChange`, `qtyAfter`, chứng từ tham chiếu và audit user.
  - **`reverseMovement(originalLedgerId, actorUserId, note?)`**:
    - Đọc bút toán gốc, kiểm tra chống đảo ngược một bút toán vốn là reversal.
    - Đảo dấu `qtyChange = -original.qtyChange`, cập nhật tồn kho an toàn bằng optimistic lock và chèn dòng ledger đảo mới trỏ `reversalOfId`.
    - **Tách biệt ngoại lệ nghiệp vụ**: Phân định rõ ràng `AlreadyReversedException` (HTTP 422 Unprocessable Entity - lỗi nghiệp vụ vĩnh viễn, không retry) thay vì dùng chung `ConflictException` (HTTP 409 - lỗi tranh chấp tạm thời, có thể retry) để ngăn chặn bug client retry vô tận.
    - **Bảo vệ chống đảo 2 lần đồng thời ở tầng DB**: Tạo migration `20260930030716_add_unique_reversal_of_id` với partial unique index `stock_ledger_reversal_of_id_uk` trên `(reversal_of_id) WHERE reversal_of_id IS NOT NULL`, bắt mã lỗi `P2002` từ PostgreSQL engine để chuyển đổi thành `AlreadyReversedException`.

* **Xây dựng 2 API Tra cứu Tồn kho & Sổ cái**:
  - **`GET /api/inventory`**: Lọc kết hợp AND theo `warehouseId`, `locationId`, `skuId`, `lotNo`. Trả về đầy đủ 9 trường dữ liệu (`skuCode`, `locationCode`, `lotNo`, `serialNo`, `qtyOnHand`, `qtyReserved`, `qtyAvailable`, `locationPurpose`, `isPickable`). Giữ nguyên trạng thái `locationPurpose` (QC, STORAGE, DAMAGED) và `isPickable` từ bảng locations để Frontend tự quyết định tổng hợp, không lọc bỏ ở backend nhằm phục vụ kiểm định viên xem hàng khu QC.
  - **`GET /api/stock-ledger`**: Lọc theo `skuId`, `locationId`, `warehouseId`. Sắp xếp mặc định `createdAt DESC` (mới nhất trước). Trả về đầy đủ các thông tin biến động và audit log.
  - **Phân trang an toàn (Safe Pagination)**: Xử lý triệt để 2 trường hợp biên — không có kết quả (`items: []`, `totalItem: 0`, `totalPage: 0`) và offset vượt trang (trang 999 trả về `items: []`) mà không gây crash hoặc lỗi `undefined`.

* **Kiểm thử toàn diện & Đảm bảo chất lượng (Verification & Testing)**:
  - Viết bộ Jest unit tests (`inventory-ledger.service.spec.ts`) bao phủ đầy đủ các hàm service và edge cases phân trang — vượt qua 15/15 tests (`npm run test`). Sửa lỗi greeting message trong `app.controller.spec.ts`.
  - Xây dựng script tích hợp database độc lập `scripts/run-test-scenarios.ts` tích hợp **Safety Guard** kiểm tra `DATABASE_URL` bắt buộc trỏ về `localhost` / `127.0.0.1` trước khi thực thi để bảo vệ dữ liệu.
  - Kiểm thử trực tiếp 9 kịch bản trên PostgreSQL container: kiểm tra chặn số âm/0, mã movement không tồn tại, mô phỏng race condition version mismatch, lệch kho, tạo mới inventory, bút toán đảo, và **bắn 2 request đảo song song đồng thời (`Promise.allSettled`)** — xác nhận đúng 1 request thành công và 1 request bị chặn bằng `AlreadyReversedException`, database chỉ ghi nhận duy nhất 1 dòng reversal.
  - Biên dịch dự án hoàn tất với `npm run build` đạt mã thoát 0 (0 drift, 0 lint/compilation errors).

* **Quản trị Nhánh Git & Khởi tạo Pull Request**:
  - Tuân thủ nghiêm ngặt quy tắc an toàn Git của nhóm: không thao tác commit hay force-push đè lên nhánh `PEEP2`.
  - Tạo nhánh tính năng mới `feature/inventory-ledger-service`, phân chia thành 3 commit độc lập chuẩn Conventional Commits:
    1. `fix(test)`: Đồng bộ hóa greeting test của AppController.
    2. `feat(inventory)`: `InventoryLedgerService`, optimistic locking, trigger append-only, migration partial unique index và script test.
    3. `feat(inventory)`: 2 API tra cứu tồn kho / sổ cái và DTOs phân trang.
  - Đẩy nhánh lên remote và tạo thành công **Pull Request #3** trỏ vào nhánh `PEEP2` kèm báo cáo kiểm thử chi tiết và ghi chú phân quyền cho Domain A.

### 2. Kế hoạch tiếp theo (Next Steps)

* Theo dõi quá trình review và phối hợp merge Pull Request #3 vào nhánh `PEEP2`.
* Phối hợp với Domain A (Nam Nguyen) để gắn middleware phân quyền (`ProtectGuard` / role permissions) vào 2 API tra cứu sau khi cơ chế RBAC sẵn sàng (thay thế decorator `@Public()` tạm thời).
* Chuẩn bị triển khai các nghiệp vụ luồng giao dịch nâng cao tiếp theo (Inbound Receipt, Putaway, Pick/Ship, Allocation/Reservation stock).
