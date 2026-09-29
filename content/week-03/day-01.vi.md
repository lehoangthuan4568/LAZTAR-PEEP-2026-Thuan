+++
title = "Ngày 01 - 28/09/2026 (Remote)"
weight = 1
+++

# BÁO CÁO THỰC HÀNH — NGÀY 01

## Domain D – Inventory Model: Hoàn thiện Thiết kế & Đánh giá Tích hợp

### 1. Chi tiết thực hiện & Kết quả đạt được

* **Hoàn thiện thiết kế (Design finalized)**: Hoàn thành và chốt ERD, Data Dictionary cùng các Quy tắc nghiệp vụ (11 quy tắc, BR-D-01 → BR-D-11) cho Domain D — Inventory Model. Bao gồm logic tính toán `qty_on_hand`/`qty_reserved`/`qty_available`, cơ chế optimistic locking thông qua `version`, và ràng buộc unique constraint theo bộ `(location_id, sku_id, lot_no/serial_no)`.
* **Đồng bộ liên Domain (Cross-domain sync)**: Rà soát và căn chỉnh lại thiết kế Inventory với Domain B (Cấu trúc kho — phân cấp BIN, mục đích sử dụng, `is_pickable`) và Domain C (Sản phẩm & Nhà cung cấp — đơn vị tính cơ sở base UOM, quản lý theo lô lot/serial tracking) sau khi ERD của các bên được công bố; cập nhật kiểu dữ liệu PK/FK sang `BIGINT` để khớp với hiện trạng triển khai thực tế của đội ngũ.
* **Đánh giá tích hợp hệ thống (Integration review)**: Xem xét ERD tổng hợp toàn hệ thống (`erd_all`) và đối chiếu chéo các deliverable từ Domain A và Domain E; phát hiện 2 vấn đề tồn đọng cần trao đổi tiếp với team:
  1. Cách xử lý vị trí (location) cho hàng đang trên đường vận chuyển (in-transit stock) trong quá trình điều chuyển kho (warehouse transfer).
  2. Cơ chế phân tách trách nhiệm (segregation of duties: người đề xuất khác người phê duyệt - proposer ≠ approver) đối với nghiệp vụ điều chỉnh tồn kho (inventory adjustment) — hiện chưa được phản ánh trong thiết kế cuối cùng nộp lên.
* **Cấu hình Repo & Môi trường phát triển**: Nhận quyền truy cập repository, clone mã nguồn, cấu hình file `.env` và khởi chạy cơ sở dữ liệu PostgreSQL cục bộ qua Docker theo hướng dẫn của team — cam kết không thao tác chỉnh sửa trực tiếp trên cơ sở dữ liệu development dùng chung.
* **Đánh giá API/DTO Tuần 2**: Rà soát đặc tả API & DTO Tuần 2 cho Domain D/E và cảnh báo một lỗ hổng triển khai quan trọng — endpoint `/api/inventory/adjust` hiện đang commit trực tiếp thay đổi mà không thông qua luồng phê duyệt — cần xử lý khắc phục trước khi bắt đầu viết backend.

### 2. Kế hoạch tiếp theo (Next Steps)

* Triển khai Prisma schema cho `inventory`/`stock_ledger` và xây dựng `StockLedgerService` (đảm bảo ghi dữ liệu trong single transaction, thực thi optimistic locking, cam kết nguyên tắc append-only bất biến), đồng thời phối hợp với phụ trách Domain A/B/C/E về các ràng buộc khóa ngoại (FK) và phân quyền còn lại.
