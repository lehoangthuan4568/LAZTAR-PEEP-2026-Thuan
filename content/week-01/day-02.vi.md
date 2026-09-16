+++
title = "Ngày 02 - Xây dựng và Triển khai Portfolio cá nhân với Next.js"
weight = 2
+++

# BÁO CÁO THỰC HÀNH — NGÀY 02

## 1. Mục tiêu công việc
* Khởi tạo và cấu hình dự án Next.js 16 kết hợp React 19 và Tailwind CSS.
* Xây dựng giao diện trang cá nhân (Portfolio) theo phong cách thiết kế Editorial, tập trung vào trải nghiệm người dùng (UX) và tính thẩm mỹ (UI).
* Thực hiện thiết kế đáp ứng (Responsive Design) cho đa thiết bị.
* Vận dụng Git/GitHub để quản lý mã nguồn theo chuẩn làm việc nhóm.
* Triển khai (Deploy) dự án lên nền tảng Vercel.

## 2. Quá trình thực hiện
* **Thiết lập dự án:** Sử dụng `create-next-app` để khởi tạo cấu trúc thư mục chuẩn. Tích hợp các biến CSS (CSS Variables) toàn cục nhằm quản lý hệ thống màu sắc và kiểu chữ một cách nhất quán.
* **Phát triển giao diện (UI Development):**
  * Áp dụng bố cục lưới (Grid Layout) và Flexbox để sắp xếp các thành phần một cách khoa học.
  * Tích hợp hiệu ứng hạt (paper grain texture) và typography Serif (Playfair Display) để tạo chiều sâu và phong cách cá nhân hóa.
  * Phân chia mã nguồn thành các Component độc lập (`Hero`, `Projects`, `Now`, `Contact`, `Footer`) nhằm tối ưu hóa khả năng tái sử dụng.
* **Tối ưu hóa tương tác (Micro-interactions):** Triển khai các hiệu ứng mượt mà khi cuộn trang (Intersection Observer) và các trạng thái di chuột (hover states) bằng CSS Transition.
* **Triển khai (Deployment):** Liên kết kho lưu trữ GitHub với Vercel, thiết lập quy trình tích hợp liên tục (CI/CD) để tự động hóa quá trình xuất bản trang web.

## 3. Tổng kết kiến thức đã học
* **Kiến trúc Next.js (App Router):** Nắm vững phương pháp tổ chức thư mục, định tuyến (routing) và cơ chế render (Server Components vs Client Components) của Next.js thế hệ mới.
* **Thiết kế đáp ứng (Responsive Web Design):** Thành thạo việc áp dụng các utility classes của Tailwind CSS (`sm:`, `md:`, `lg:`) để điều chỉnh bố cục, kích thước phông chữ và khoảng cách tương thích với kích thước màn hình di động.
* **Quản lý quy trình làm việc Git:** Áp dụng hiệu quả chiến lược phân nhánh (`feature/trang-ca-nhan`), cấu trúc lịch sử commit tuyến tính và rõ ràng theo quy chuẩn Conventional Commits.
* **Quy trình triển khai đám mây:** Hiểu rõ cơ chế hoạt động của nền tảng PaaS (Platform as a Service) như Vercel, từ bước cấp quyền truy cập mã nguồn đến việc biên dịch (build) và phân phối tĩnh (static hosting).

## 4. Khó khăn gặp phải & Phương hướng giải quyết
* **Khó khăn 1: Trùng lặp mã nguồn và cấu trúc tệp (Component duplication)**
  * *Vấn đề:* Ban đầu, việc quản lý nội dung tĩnh trực tiếp bên trong các Component dẫn đến sự cồng kềnh và khó bảo trì.
  * *Giải pháp:* Tách biệt dữ liệu thành file cấu hình độc lập (`data/profile.ts`). Áp dụng mô hình hướng dữ liệu (Data-driven approach) để render giao diện thông qua phương thức `Array.map()`.
* **Khó khăn 2: Quản lý tính trạng thái cuộn trang (Scroll state detection)**
  * *Vấn đề:* Cần hiển thị các thành phần động dựa trên vị trí cuộn của người dùng (ví dụ: Thanh điều hướng làm mờ, nút cuộn lên đầu trang) nhưng cần đảm bảo hiệu suất.
  * *Giải pháp:* Sử dụng `useEffect` kết hợp với Event Listener (`passive: true`) và `Intersection Observer API` để tối ưu hóa hiệu năng DOM thay vì lắng nghe sự kiện cuộn một cách liên tục.
