+++
title = "Ngày 04 - 18/09/2026 (Office)"
weight = 4
+++

# BÁO CÁO THỰC HÀNH — NGÀY 04

## 1. Mục tiêu công việc
* Nâng cấp giao diện trang Portfolio cá nhân lên chuẩn "Premium Utilitarian Minimalism" (Tối giản cao cấp).
* Tích hợp thư viện `framer-motion` (thông qua `motion/react`) để thay thế các hàm intersection observer tự viết, tạo hiệu ứng xuất hiện mượt mà.
* Cải thiện không gian hiển thị (macro-whitespace) và hệ thống phân cấp typography mang phong cách tạp chí (editorial).
* Thay thế toàn bộ emoji thành icon vector chuyên nghiệp (`lucide-react`).

## 2. Chi tiết thực hiện

### A. Tái cấu trúc UI/UX
- **Anti-Slop Design:** Lược bỏ các xu hướng thiết kế rườm rà như đổ bóng quá đậm, bo tròn góc quá mức (`rounded-full`) hay hiệu ứng dải màu (gradient) không cần thiết.
- **Bento Grid:** Thiết kế lại phần Kỹ năng (Skills) thành dạng lưới bento sắc nét với viền mỏng 1px (`var(--border)`).
- **Micro-interactions:** Sử dụng hiệu ứng ấn vật lý (`scale: 0.98`) khi người dùng tương tác thay vì dùng bóng đổ lớn.

### B. Chuyển động & Hiệu suất
- Áp dụng `motion.div` kết hợp `whileInView` giúp các phần tử xuất hiện tuần tự (staggered reveal) một cách nhẹ nhàng và không gây giật lag.
- Đảm bảo các hiệu ứng chỉ tác động lên `transform` và `opacity` để được tối ưu hóa bởi GPU.

## 3. Kết quả đạt được
- Hoàn thành giao diện mới tinh gọn, chuyên nghiệp hơn mà vẫn giữ nguyên tông màu Warm Cream ban đầu.
- Đã commit và push toàn bộ code lên nhánh `feature/trang-ca-nhan` trên Github.
- Trang web đã được Vercel tự động deploy thành công.

## 4. Tài liệu tham khảo
- [Tài liệu Framer Motion](https://motion.dev/)
- [Thư viện Lucide Icons](https://lucide.dev/)
