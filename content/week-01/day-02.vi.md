+++
title = "Ngày 02 - Lý thuyết React & Next.js và Thực hành Landing Page"
weight = 2
+++

# BÁO CÁO THỰC HÀNH — NGÀY 02

## A. CƠ SỞ LÝ THUYẾT

### Phần 1. React cơ bản 

**1. React là gì?**
React là một thư viện JavaScript mã nguồn mở do Facebook phát triển, chuyên dùng để xây dựng giao diện người dùng (UI), đặc biệt là cho các ứng dụng trang đơn (SPA). React hoạt động dựa trên luồng dữ liệu một chiều và sử dụng Virtual DOM để tối ưu hóa hiệu suất hiển thị.

**2. Component trong React là gì? Có mấy loại component?**
Component là các khối xây dựng cơ bản trong React, cho phép chia nhỏ giao diện thành các thành phần độc lập, có thể tái sử dụng.
Có hai loại chính:
- **Class Components:** Các class ES6 kế thừa từ `React.Component`, có khả năng quản lý state và lifecycle.
- **Functional Components:** Các hàm JavaScript đơn giản nhận props và trả về React elements. Kể từ React 16.8, với sự ra đời của Hooks, Functional Components đã trở thành tiêu chuẩn do tính ngắn gọn và hiệu quả.

**3. JSX là gì?**
JSX (JavaScript XML) là một phần mở rộng cú pháp cho JavaScript, cho phép viết mã HTML trực tiếp bên trong các file JavaScript. JSX giúp mã nguồn dễ đọc hơn và được Babel biên dịch thành các lệnh gọi `React.createElement()` dưới nền.

**4. Props là gì?**
Props (Properties) là cơ chế để truyền dữ liệu từ component cha xuống component con. Props có tính chất read-only (chỉ đọc) và component con không thể tự thay đổi giá trị của props mà nó nhận được.

**5. State là gì? State khác Props như thế nào?**
- **State** là dữ liệu nội bộ của một component, có thể thay đổi theo thời gian thông qua các tương tác của người dùng hoặc các phản hồi từ hệ thống. Khi state thay đổi, component sẽ tự động re-render.
- **Khác biệt:** Props được truyền từ ngoài vào (cha truyền cho con) và không thể bị sửa đổi bởi component nhận, trong khi State được quản lý cục bộ bên trong component và có thể bị sửa đổi bởi chính component đó.

**6. Virtual DOM là gì? Vì sao React sử dụng Virtual DOM?**
Virtual DOM là một bản sao ảo của Real DOM được lưu trong bộ nhớ. 
React sử dụng Virtual DOM để tối ưu hóa quá trình cập nhật giao diện. Khi state hoặc props thay đổi, React tạo ra một Virtual DOM mới, so sánh (Diffing) với Virtual DOM cũ để tìm ra những sự khác biệt, và chỉ cập nhật (Patching) những phần thực sự thay đổi lên Real DOM, giúp giảm thiểu các thao tác tốn kém trên trình duyệt.

**7. Hooks là gì? Kể tên một số Hook phổ biến trong React.**
Hooks là các hàm đặc biệt được giới thiệu trong React 16.8, cho phép Functional Components sử dụng state và các tính năng khác của React (như lifecycle) mà không cần viết Class.
Các Hook phổ biến: `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`.

**8. `useState` dùng để làm gì?**
Dùng để khai báo và quản lý state cục bộ trong Functional Component. Nó trả về một mảng gồm hai phần tử: giá trị state hiện tại và một hàm để cập nhật giá trị đó.

**9. `useEffect` dùng để làm gì?**
Dùng để thực hiện các side effects (tác vụ phụ) trong Functional Component, chẳng hạn như fetching dữ liệu, thiết lập subscriptions, hoặc thao tác thủ công với DOM. Nó thay thế cho các lifecycle methods như `componentDidMount`, `componentDidUpdate`, và `componentWillUnmount`.

**10. Lifecycle của một React Component gồm những giai đoạn nào?**
- **Mounting:** Khi component được tạo ra và chèn vào DOM.
- **Updating:** Khi component bị re-render do state hoặc props thay đổi.
- **Unmounting:** Khi component bị gỡ bỏ khỏi DOM.

**11. Client-Side Rendering (CSR) là gì?**
CSR là kỹ thuật render giao diện ở phía client (trình duyệt). Ban đầu, server chỉ gửi về một trang HTML rỗng cùng với các file JavaScript. Sau khi trình duyệt tải và thực thi JavaScript, giao diện mới được vẽ ra (render) hoàn chỉnh.

**12. React Router là gì?**
React Router là thư viện tiêu chuẩn để quản lý định tuyến (routing) trong các ứng dụng React. Nó cho phép điều hướng giữa các views/pages khác nhau trong một SPA mà không cần phải tải lại trang (reload).

**13. React thuần có hỗ trợ Routing, SEO và API Server không?**
Không. React thuần chỉ là một thư viện UI. Để có Routing (phải dùng `react-router-dom`), SEO tốt (cần cơ chế SSR/SSG), và API Server (cần backend riêng), nhà phát triển phải kết hợp với các thư viện hoặc framework khác (như Next.js).

**14. Context API là gì? Khi nào nên sử dụng?**
Context API là một cơ chế cho phép truyền dữ liệu xuyên qua cây component mà không cần phải truyền props thủ công qua từng cấp (tránh "props drilling"). 
Nên sử dụng khi có các dữ liệu toàn cục (global) cần được chia sẻ bởi nhiều component ở các mức độ khác nhau, ví dụ: theme (dark/light), thông tin user đăng nhập, ngôn ngữ.

**15. SPA (Single Page Application) là gì?**
Là ứng dụng web chỉ có một trang HTML duy nhất. Thay vì tải lại toàn bộ trang từ server khi người dùng điều hướng, SPA chỉ tải dữ liệu (thường qua API) và sử dụng JavaScript để render lại các thành phần giao diện cần thiết, mang lại trải nghiệm mượt mà như ứng dụng desktop.


### Phần 2. So sánh React và Next.js

**1. Next.js là gì?**
Next.js là một React framework mã nguồn mở được phát triển bởi Vercel. Nó cung cấp kiến trúc và các công cụ tối ưu hóa (như SSR, SSG, Routing tích hợp) để xây dựng các ứng dụng web React chuẩn production.

**2. Điểm khác biệt cốt lõi giữa React và Next.js là gì?**
- React là một thư viện (library) chuyên về UI (CSR).
- Next.js là một framework bao bọc React, cung cấp giải pháp toàn diện bao gồm cả Server-side Rendering (SSR), File-based routing, SEO optimization và API Routes.

**3. Routing trong React và Next.js khác nhau như thế nào?**
- React: Phải cấu hình thủ công qua code bằng thư viện bên thứ 3 (`react-router-dom`).
- Next.js: Sử dụng File-based Routing. Hệ thống tự động tạo các route dựa trên cấu trúc thư mục (trong thư mục `pages` hoặc `app`).

**4. Rendering trong React và Next.js khác nhau ra sao?**
- React: Mặc định là Client-Side Rendering (CSR).
- Next.js: Hỗ trợ đa dạng các chiến lược rendering: Server-Side Rendering (SSR), Static Site Generation (SSG), Incremental Static Regeneration (ISR), và cả CSR.

**5. Vì sao Next.js hỗ trợ SEO tốt hơn React thuần?**
Vì Next.js có thể render HTML hoàn chỉnh ngay từ Server (SSR/SSG). Khi bot của các công cụ tìm kiếm (Googlebot) thu thập dữ liệu, chúng sẽ nhận được file HTML đầy đủ nội dung để index, trái ngược với CSR của React (chỉ trả về file HTML rỗng chờ JS chạy).

**6. Hiệu năng tải trang đầu tiên (First Load) của React và Next.js khác nhau như thế nào?**
Next.js có First Load nhanh hơn đáng kể vì server trả về HTML đã được render sẵn, người dùng có thể xem nội dung ngay lập tức trong khi chờ JS được tải và hydrate. React tốn thời gian chờ tải JS và thực thi xong mới hiển thị giao diện.

**7. Cấu trúc dự án React và Next.js khác nhau ra sao?**
Dự án React (như Create React App hoặc Vite) thường có thư mục `src` tự do. Next.js ép buộc một số cấu trúc nhất định để framework tự động hóa tính năng, đặc biệt là thư mục `app` (App Router) hoặc `pages` cho định tuyến.

**8. Next.js có thay thế React không? Vì sao?**
Không. Next.js được xây dựng *trên nền tảng* React. Next.js cần React để xử lý việc quản lý UI (Components, State, Hooks). Nó chỉ bổ sung thêm các tính năng về kiến trúc mà React thuần còn thiếu.

**9. Khi nào nên dùng React thuần và khi nào nên dùng Next.js?**
- **React thuần:** Cho các ứng dụng nội bộ (Dashboard, Admin panel) nơi SEO không quan trọng và tính tương tác phía client cao.
- **Next.js:** Cho các website public cần SEO mạnh (E-commerce, Landing page, Blog) và cần tối ưu tốc độ tải trang (Web Vitals).


### Phần 3. Next.js

**1. App Router và Pages Router trong Next.js là gì?**
- **Pages Router:** Mô hình định tuyến cũ của Next.js (trước bản 13), dựa trên thư mục `pages`.
- **App Router:** Mô hình mới (từ Next.js 13), hỗ trợ Server Components mặc định, tối ưu nested layouts, và cơ chế fetching dữ liệu hiện đại, dựa trên thư mục `app`.

**2. Server Component và Client Component khác nhau như thế nào?**
- **Server Component:** Chạy và render trên Server. Không thể sử dụng Hooks (useState, useEffect) hoặc các event listener (onClick). Giúp giảm kích thước JS gửi xuống client.
- **Client Component:** Chạy trên Client (hoặc hydrate trên client). Có thể dùng Hooks và tương tác UI. Trong App Router, phải khai báo `"use client"` ở đầu file.

**3. SSR (Server-Side Rendering) là gì?**
Giao diện được render lại thành HTML trên server *mỗi khi có một request* từ người dùng, đảm bảo dữ liệu luôn mới nhất.

**4. SSG (Static Site Generation) là gì?**
Giao diện được render sẵn thành HTML *tại thời điểm build time*. File HTML này được tái sử dụng cho mọi request, giúp tốc độ tải cực kỳ nhanh.

**5. ISR (Incremental Static Regeneration) là gì?**
Kỹ thuật cho phép cập nhật lại một trang tĩnh (SSG) ở background sau khi trang đã được build, theo một chu kỳ thời gian nhất định (ví dụ: mỗi 60 giây), giúp dữ liệu không bị lỗi thời mà vẫn giữ được tốc độ của SSG.

**6. File-based Routing trong Next.js hoạt động như thế nào?**
Các route của ứng dụng được ánh xạ trực tiếp từ cây thư mục. Ví dụ: file `app/about/page.tsx` sẽ tự động tạo ra đường dẫn `/about`.

**7. Dynamic Route trong Next.js là gì?**
Là các route nhận tham số động, được định nghĩa bằng dấu ngoặc vuông `[]`. Ví dụ: `app/blog/[id]/page.tsx` sẽ khớp với `/blog/1`, `/blog/2`... Tham số `id` có thể được lấy ra trong component.

**8. layout.tsx trong App Router dùng để làm gì?**
Dùng để tạo ra một giao diện bao bọc (wrapper) dùng chung cho một hoặc nhiều trang bên dưới nó (ví dụ: Header, Footer, Sidebar), giúp giao diện không bị re-render khi chuyển trang.

**9. API Routes (Route Handlers) trong Next.js là gì?**
Tính năng cho phép xây dựng các endpoint API (Backend) trực tiếp bên trong dự án Next.js (thường viết trong `app/api/route.ts`).

**10. getStaticProps và getServerSideProps là gì? Chúng dùng trong trường hợp nào?**
Đây là các hàm lấy dữ liệu trong mô hình **Pages Router** cũ.
- `getStaticProps`: Lấy dữ liệu lúc build-time (phục vụ SSG).
- `getServerSideProps`: Lấy dữ liệu ở mỗi request (phục vụ SSR).
*(Trong App Router mới, các hàm này đã được thay thế bằng native `fetch()` API).*

**11. next/image giúp tối ưu hình ảnh như thế nào?**
Component `<Image />` tự động: 
- Nén ảnh và chuyển đổi định dạng hiện đại (WebP/AVIF).
- Lazy loading (chỉ tải ảnh khi cuộn tới).
- Responsive tự động dựa trên kích thước thiết bị.
- Ngăn chặn hiện tượng Cumulative Layout Shift (CLS).

**12. Middleware trong Next.js là gì?**
Là một đoạn code chạy trước khi một request được hoàn thành. Thường dùng để xử lý xác thực (Authentication), chuyển hướng (Redirects), hoặc ghi log trước khi người dùng truy cập vào một trang cụ thể.

**13. Làm thế nào để điều hướng giữa các trang trong Next.js?**
Sử dụng component `<Link href="...">` thay vì thẻ `<a>` thuần, giúp Next.js pre-fetch dữ liệu và chuyển trang theo kiểu SPA (không tải lại toàn bộ trang). Hoặc dùng Hook `useRouter()`.

**14. Metadata và SEO trong Next.js được xử lý như thế nào?**
Trong App Router, sử dụng biến export `metadata` hoặc hàm `generateMetadata()` trong các file `layout.tsx` hoặc `page.tsx` để quản lý các thẻ meta (title, description, open graph) linh hoạt.

**15. Next.js có hỗ trợ TypeScript không?**
Có, hỗ trợ built-in (mặc định) và rất tuyệt vời. Next.js tự động cấu hình `tsconfig.json` khi khởi tạo và cung cấp hệ thống định kiểu (typings) chặt chẽ cho toàn bộ framework.

**16. Có thể deploy dự án Next.js lên những nền tảng nào?**
Tốt nhất là Vercel (nền tảng mẹ của Next.js). Ngoài ra có thể deploy lên AWS, Google Cloud, DigitalOcean, Netlify, hoặc tự cấu hình qua Docker container.

---

## B. THỰC HÀNH: PHÁT TRIỂN LANDING PAGE DOANH NGHIỆP LAZTAR

### 1. Tổng quan
Thực hành xây dựng trang Landing Page giới thiệu doanh nghiệp cho công ty LAZTAR, yêu cầu tính chuyên nghiệp cao, sử dụng Next.js (App Router), Tailwind CSS và Framer Motion.

### 2. Những gì đã học được
- **Kiến trúc UI/UX Doanh nghiệp (Enterprise Design):** Áp dụng phong cách thiết kế **Swiss Minimalist & Editorial**, sử dụng các khối thẻ lưới (Bento grid), loại bỏ các yếu tố rườm rà (glowing orbs, heavy gradients) để tạo sự sang trọng, đáng tin cậy.
- **Micro-interactions cao cấp:** Tích hợp `framer-motion` để xử lý các hiệu ứng cuộn trang (scroll-triggered stagger fade-up) và hiệu ứng hover tinh tế, khác biệt hoàn toàn với các CSS transitions cơ bản.
- **Xử lý tài nguyên tĩnh:** Tích hợp bộ icon chuẩn (Lucide React) thay cho SVGs tĩnh để dễ dàng scale kích thước và thay đổi màu sắc. Cấu hình `next.config.ts` cho phép tải ảnh từ external domains (Unsplash).

### 3. Khó khăn gặp phải
- **Thiết kế ban đầu thiếu chiều sâu ("AI-generated look"):** Ở những lần lặp đầu tiên, giao diện mang cảm giác thiết kế hàng loạt, sử dụng màu gradient neon và bo góc quá đà, làm mất đi tính nghiêm túc của một công ty B2B.
- **Lỗi tương thích thư viện Icon:** Gặp lỗi build do import các brand icons (Facebook, LinkedIn) từ `lucide-react` trong phiên bản mới (thư viện đã gỡ bỏ các icon thương hiệu).

### 4. Cách giải quyết
- Áp dụng các nguyên tắc thiết kế từ bộ kỹ năng **ui-ux-pro-max**, chuẩn hóa lại hệ màu (Navy/Trắng), sử dụng font Playfair Display cho tiêu đề lớn, thay đổi bố cục floating/masonry thành các grid thẳng thớm, chuyên nghiệp.
- Để khắc phục lỗi `lucide-react`, tiến hành nhúng trực tiếp mã nguồn SVG của các mạng xã hội vào component Footer, đảm bảo tính ổn định và không phụ thuộc vào cập nhật thư viện bên thứ ba.
