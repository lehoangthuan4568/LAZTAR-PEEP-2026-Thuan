+++
title = "Ngày 03 - 01/10/2026 (Remote)"
weight = 3
+++

# BÁO CÁO THỰC HÀNH — NGÀY 03

## Domain D & E – Security Hardening, Stock Ledger Reversal API, Pickable Inventory Summary & Conflict Resolution

### 1. Chi tiết thực hiện & Kết quả đạt được

* **Bảo mật Cấp Database Engine (Block TRUNCATE on `stock_ledger`)**:
  - Tạo và thực thi Prisma migration `20261001062000_block_truncate_stock_ledger` trên PostgreSQL cục bộ.
  - Cài đặt trigger cấp câu lệnh (statement-level trigger) `trg_prevent_stock_ledger_truncate` trên bảng `stock_ledger` kích hoạt hàm `prevent_stock_ledger_mutation()`.
  - Kết hợp với trigger cấp dòng (row-level) đã có từ trước, đảm bảo bảng `stock_ledger` đạt chuẩn bất biến tuyệt đối (immutable, append-only) ở tầng database engine: chặn hoàn toàn mọi thao tác `UPDATE`, `DELETE` (DML) và `TRUNCATE` (DDL), kể cả từ các kết nối quản trị trực tiếp hoặc ORM.

* **Thắt chặt An toàn API Tra cứu Kho (ProtectGuard Integration)**:
  - Gỡ bỏ hoàn toàn decorator `@Public()` khỏi các endpoint tra cứu dữ liệu kho tại `InventoryController` (`GET /api/inventory`) và `StockLedgerController` (`GET /api/stock-ledger`).
  - Đưa 2 API này về cơ chế bảo vệ mặc định của `ProtectGuard` toàn cục (được khởi tạo từ `main.ts` và tích hợp với hệ thống Auth/RBAC của Domain A).
  - Xác nhận an toàn: Yêu cầu bắt buộc phải có JWT Bearer token hợp lệ trong header; chặn đứng các request không xác thực với mã lỗi `401 Unauthorized`.
  - Đóng gói toàn bộ phần nâng cấp an toàn vào nhánh `PEEP2-feature/inventory-security-hardening`, mở **Pull Request #8** trỏ vào `PEEP2`. PR đã được tech lead (`MinhTrungPham`) review và merge thành công vào nhánh chính.

* **Xây dựng API Đảo Bút toán Sổ cái (`POST /api/stock-ledger/:id/reverse`)**:
  - Expose hàm nghiệp vụ `InventoryLedgerService.reverseMovement()` ra giao diện REST API.
  - **Tham số & DTO**: Nhận tham số route `:id` (ID dòng ledger gốc cần đảo) và body tùy chọn `ReverseStockLedgerBodyDto` (`{ note?: string }`). Tự động trích xuất `actorUserId` từ `req.user.id` (token đã decode qua guard).
  - **Phân quyền vai trò quản lý (Role-Based Authorization)**: Chỉ cấp quyền thực hiện bút toán đảo cho các vai trò quản trị/giám sát kho gồm `ADMIN`, `WH_MANAGER` (tên role chính thức chuẩn hóa theo Domain A) và `SUPERVISOR`. Chặn các vai trò tác nghiệp hoặc xem (`VIEWER`, `PICKER`...) với mã lỗi `403 Forbidden`. Đặt ghi chú kỹ thuật `TODO` sẵn sàng chuyển sang generic permission khi Domain A phát hành phân quyền hạt nhân.
  - **Xử lý ngoại lệ chuẩn RESTful**: Bắt chính xác `404 Not Found` (ledger không tồn tại), `400 Bad Request` (cố tình đảo 1 dòng vốn là reversal), và `422 Unprocessable Entity` (`AlreadyReversedException` khi cố tình đảo ngược 2 lần một bút toán).
  - Trả về dòng ledger reversal mới tạo với đầy đủ thông tin đồng bộ (`movementTypeCode: "REVERSAL"`, `reversalOfId`, `qtyChange` đảo dấu).

* **Xây dựng API Tổng hợp Tồn kho Khả dụng theo SKU (`GET /api/inventory/summary`)**:
  - Giải quyết bài toán nghiệp vụ cốt lõi: Ngăn chặn rủi ro Frontend tự cộng dồn tất cả các dòng tồn kho gây tính nhầm hàng đang nằm ở khu kiểm tra chất lượng (QC) hoặc hàng hỏng (DAMAGED) vào số lượng bán được.
  - **Cơ chế Lọc Vị trí Soạn hàng (Pickable-Location Filtering)**: Chỉ tính tổng (`qtyOnHand`, `qtyReserved`, `qtyAvailable`) từ các dòng inventory thuộc vị trí có `location.isPickable = true` (khu lưu trữ STORAGE hoặc PICKING).
  - **Thông tin Minh bạch Vị trí Bị loại**: Trả về trường `excludedLocationsCount` (số dòng tồn kho bị loại do `isPickable = false`), giúp nhân viên vận hành và Frontend biết rõ còn hàng đang nằm tại khu QC/sửa chữa mà không gây hiểu nhầm là "thất thoát hàng hóa".
  - **Xử lý An toàn & Biên**: Bắt buộc truyền `skuId` (ném `400 Bad Request` nếu thiếu). Hỗ trợ lọc thêm theo `warehouseId`. Nếu SKU chưa phát sinh tồn kho hoặc chỉ có hàng non-pickable, API trả về các tổng bằng `0` an toàn thay vì crash hay ném lỗi.

* **Kiểm thử Toàn diện & Giải quyết Xung đột Nhánh (Conflict Resolution)**:
  - **Unit Testing**: Bổ sung 4 unit test cases cho `getInventorySummary` trong `inventory-ledger.service.spec.ts`, nâng tổng số test cases của module lên **19/19 tests passing** (`npm run test`).
  - **Kiểm thử tích hợp Live API**: Kiểm thử 8 kịch bản trực tiếp trên PostgreSQL container (kiểm tra phân quyền WH_MANAGER vs VIEWER, kiểm tra đảo 2 lần đồng thời, kiểm tra tính toán tổng pickable vs QC, kiểm tra 401 khi thiếu token).
  - **Giải quyết Xung đột Git (Conflict Resolution)**: Sau khi PR #8 được merge vào `PEEP2`, nhánh PR #9 phát sinh conflict tại `inventory.controller.ts` và `stock-ledger.controller.ts`. Đã thực hiện `git merge origin/PEEP2`, giải quyết conflict sạch sẽ: giữ toàn bộ endpoint mới, xóa bỏ hoàn toàn `@Public()`, rà soát triệt để không để sót bất kỳ unused import hay unused decorator nào.
  - **Làm rõ ranh giới nhánh đa nhóm & Reopen PR #9**: Làm rõ việc thành viên nhóm 1 (`PEEP1`) bấm nhầm close PR #9 do nhầm lẫn giữa hai nhóm trong repo chung `PEEP2026PART2`. Đã sync các commit mới nhất từ `origin/PEEP2` (sau khi PR #10 FE và PR #11 UOM conversion được merge), biên dịch thành công (`npm run build` mã 0) và reopen thành công PR #9 ở trạng thái `CLEAN` và `MERGEABLE`.

### 2. Kế hoạch tiếp theo (Next Steps)

* Theo dõi quá trình review và phối hợp cùng reviewer (`MinhTrungPham`) để merge Pull Request #9 vào nhánh `PEEP2`.
* Phối hợp với Frontend (Team PEEP2 sau khi hoàn thiện khung giao diện ở PR #10) để kết nối màn hình hiển thị tồn kho chi tiết, widget tổng hợp tồn khả dụng pickable và chức năng tạo bút toán đảo sổ cái có phân quyền.
* Chuẩn bị triển khai các module nghiệp vụ nâng cao theo lộ trình Mini-WMS: Quản lý vị trí nâng cao (Location Capacity / Bin Hierarchy), luồng nhập kho Inbound Putaway và xuất kho Outbound Allocation.
