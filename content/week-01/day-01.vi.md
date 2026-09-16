+++
title = "Ngày 01 - Cẩm nang toàn tập Git & Làm việc nhóm"
weight = 1
+++

# TÀI LIỆU GIT THỰC HÀNH — NGÀY 01
> Tổng hợp các lệnh Git thông dụng, quy chuẩn làm việc nhóm và hướng dẫn chi tiết xử lý Conflict.

---

## 1. Cấu hình Git ban đầu

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git config --global user.name "Tên"` | Thiết lập tên hiển thị khi commit | Rất thường xuyên |
| `git config --global user.email "email"` | Thiết lập email gắn với commit (nên trùng email trên GitHub/GitLab) | Rất thường xuyên |
| `git config --list` | Xem toàn bộ cấu hình Git hiện tại | Thỉnh thoảng |
| `git config --global core.editor "code --wait"` | Đặt trình soạn thảo mặc định cho Git (VS Code) | Thỉnh thoảng |

---

## 2. Khởi tạo dự án & Clone

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git init` | Khởi tạo một repository Git mới trong thư mục hiện tại | Thỉnh thoảng |
| `git clone <url>` | Sao chép (tải về) một repository từ xa về máy | Rất thường xuyên |
| `git clone -b <branch> <url>` | Clone và checkout thẳng vào một nhánh cụ thể | Thường xuyên |

---

## 3. Kiểm tra trạng thái & thay đổi

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git status` | Xem trạng thái các file (đã thay đổi, đã stage, chưa theo dõi...) | Rất thường xuyên |
| `git diff` | So sánh thay đổi chưa stage với lần commit gần nhất | Thường xuyên |
| `git diff --staged` | So sánh thay đổi đã stage với lần commit gần nhất | Thường xuyên |
| `git log` | Xem lịch sử commit | Rất thường xuyên |
| `git log --oneline --graph --all` | Xem lịch sử commit dạng rút gọn kèm sơ đồ nhánh | Thường xuyên |
| `git show <commit>` | Xem chi tiết nội dung thay đổi của một commit | Thỉnh thoảng |
| `git blame <file>` | Xem ai đã sửa từng dòng trong file, ở commit nào | Thỉnh thoảng |

---

## 4. Thêm thay đổi & Commit

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git add <file>` | Đưa một file cụ thể vào staging area | Rất thường xuyên |
| `git add .` | Đưa toàn bộ thay đổi trong thư mục hiện tại vào staging area | Rất thường xuyên |
| `git commit -m "message"` | Lưu lại thay đổi đã stage kèm mô tả ngắn gọn | Rất thường xuyên |
| `git commit -am "message"` | Gộp `git add` (file đã theo dõi) + `commit` trong một lệnh | Thường xuyên |
| `git commit --amend` | Sửa lại nội dung/message của commit gần nhất (chưa push) | Thỉnh thoảng |

---

## 5. Làm việc với Branch (nhánh)

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git branch` | Liệt kê các nhánh hiện có trên máy | Rất thường xuyên |
| `git branch <name>` | Tạo nhánh mới nhưng chưa chuyển sang | Thường xuyên |
| `git checkout -b <name>` | Tạo nhánh mới và chuyển sang ngay lập tức | Rất thường xuyên |
| `git switch -c <name>` | Tương tự `checkout -b` (cú pháp mới, rõ nghĩa hơn) | Thường xuyên |
| `git switch <name>` | Chuyển sang nhánh đã tồn tại | Thường xuyên |
| `git branch -d <name>` | Xóa nhánh đã merge (an toàn) | Thỉnh thoảng |
| `git branch -D <name>` | Ép xóa nhánh dù chưa merge (cẩn trọng) | Hiếm / cẩn trọng |

---

## 6. Làm việc với Remote (kho từ xa)

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git remote -v` | Xem danh sách remote đang liên kết (thường là origin) | Thường xuyên |
| `git remote add origin <url>` | Gắn repository từ xa vào project local | Thỉnh thoảng |
| `git fetch` | Tải về các thay đổi mới nhất từ remote nhưng chưa merge | Rất thường xuyên |
| `git pull` | Tải về và tự động merge thay đổi từ remote vào nhánh hiện tại | Rất thường xuyên |
| `git pull --rebase` | Tải về và rebase thay vì merge (giữ lịch sử commit thẳng, sạch) | Rất thường xuyên |
| `git push` | Đẩy commit local lên remote | Rất thường xuyên |
| `git push -u origin <branch>` | Đẩy nhánh mới lên remote và thiết lập theo dõi (tracking) | Rất thường xuyên |
| `git push --force-with-lease` | Ép đẩy lên remote an toàn hơn `--force` (kiểm tra trước khi ghi đè) | Hiếm / cẩn trọng |

---

## 7. Merge & Rebase

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git merge <branch>` | Gộp nhánh khác vào nhánh hiện tại | Rất thường xuyên |
| `git rebase <branch>` | Viết lại lịch sử commit của nhánh hiện tại dựa trên nhánh khác | Thường xuyên |
| `git rebase -i HEAD~n` | Rebase tương tác để gộp/sửa/xóa n commit gần nhất | Thỉnh thoảng |
| `git rebase --continue` | Tiếp tục rebase sau khi đã xử lý xong xung đột | Thỉnh thoảng |
| `git rebase --abort` | Hủy bỏ quá trình rebase, quay về trạng thái ban đầu | Thỉnh thoảng |

---

## 8. Hướng dẫn chi tiết: Xử lý Xung đột (Conflict Resolution)

### 8.1. Xung đột (Conflict) là gì?
Xung đột xảy ra khi hai nhánh cùng chỉnh sửa trên **cùng những dòng code** của một file, hoặc một bên xóa file trong khi bên kia đang sửa. Khi thực hiện `merge` hoặc `rebase`, Git không thể tự quyết định lấy đoạn code nào nên sẽ dừng lại để lập trình viên tự xử lý.

### 8.2. Dùng gì để giải quyết? (Tools)
1. **Trình soạn thảo VS Code (Khuyên dùng - trực quan nhất):**
   * VS Code tự động nhận diện conflict và hiển thị các nút thao tác nhanh ngay trên dòng code:
     * **Accept Current Change:** Giữ nguyên code của nhánh hiện tại bạn đang đứng (HEAD).
     * **Accept Incoming Change:** Lấy code mới từ nhánh đang gộp vào.
     * **Accept Both Changes:** Giữ lại cả hai đoạn code.
     * **Resolve in Merge Editor:** Bật chế độ so sánh 3 cột chuyên sâu (Bên trái: code hiện tại, Bên phải: code kéo về, Ở giữa: kết quả sau khi ghép).
2. **Conflict Markers (Ký hiệu đánh dấu bằng mắt):**
   * Nếu xem file bằng text editor thông thường, bạn sẽ thấy các mốc:
     ```text
     <<<<<<< HEAD (Code hiện tại của bạn)
     const apiUrl = "https://api.v1.domain.com";
     =======
     const apiUrl = "https://api.v2.domain.com";
     >>>>>>> feature/new-api (Code nhánh kéo về)
     ```
3. **Công cụ Mergetool chuyên dụng:**
   * Dùng lệnh `git mergetool` để mở các phần mềm đồ họa như **Beyond Compare, Meld, KDiff3, Sourcetree** khi gặp các ca xung đột nhiều file phức tạp.

### 8.3. Sử dụng cách nào để giải quyết? (Quy trình chuẩn từng bước)
* **Bước 1: Xác định các file bị xung đột**
  Chạy lệnh để liệt kê danh sách file:
  ```bash
  git status
  ```
  *(Các file xung đột sẽ nằm ở mục Unmerged paths với trạng thái both modified).*

* **Bước 2: Mở file và giải quyết xung đột**
  * Mở các file bị lỗi trong VS Code.
  * Trao đổi nhanh với thành viên cùng sửa file (nếu làm nhóm) để thống nhất đoạn code đúng.
  * Bấm chọn Accept Current / Incoming / Both hoặc tự sửa code theo ý muốn.
  * Xóa sạch tất cả các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>`.

* **Bước 3: Đánh dấu đã xử lý xong (Stage)**
  ```bash
  git add <tên-file-đã-sửa>
  # Hoặc stage toàn bộ file đã sửa:
  git add .
  ```

* **Bước 4: Hoàn thành quá trình**
  * Nếu đang trong quá trình Merge:
    ```bash
    git commit -m "Resolve merge conflict between main and feature branch"
    ```
  * Nếu đang trong quá trình Rebase (`git pull --rebase`):
    ```bash
    git rebase --continue
    ```
    *(Lặp lại Bước 2 và 3 nếu commit tiếp theo vẫn bị xung đột).*

> ⚠️ **Lưu ý khẩn cấp - Hủy bỏ nếu làm sai (Abort):**
> * Khi đang merge mà lỡ tay làm hỏng, gõ: `git merge --abort`
> * Khi đang rebase mà muốn hủy quay về trạng thái cũ, gõ: `git rebase --abort`

---

## 9. Tạm cất thay đổi (Stash)

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git stash` | Cất tạm các thay đổi chưa commit để chuyển việc/nhánh khác | Thường xuyên |
| `git stash pop` | Lấy lại thay đổi đã cất và xóa khỏi danh sách stash | Thường xuyên |
| `git stash list` | Xem danh sách các bản stash đã lưu | Thỉnh thoảng |
| `git stash drop` | Xóa một bản stash cụ thể | Hiếm / cẩn trọng |

---

## 10. Hoàn tác & Khôi phục

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git restore <file>` | Hoàn tác thay đổi chưa stage của một file | Thường xuyên |
| `git restore --staged <file>` | Đưa file ra khỏi staging area (giữ nguyên nội dung sửa) | Thường xuyên |
| `git reset --soft HEAD~1` | Hủy commit gần nhất nhưng giữ nguyên thay đổi đã stage | Thỉnh thoảng |
| `git reset --hard HEAD~1` | Xóa hẳn commit gần nhất và toàn bộ thay đổi liên quan | Hiếm / cẩn trọng |
| `git revert <commit>` | Tạo commit mới để đảo ngược một commit cũ (an toàn cho nhánh chung) | Thường xuyên |

---

## 11. Gắn thẻ phiên bản (Tag)

| Lệnh | Ý nghĩa | Mức độ dùng trong team |
|:---|:---|:---|
| `git tag` | Liệt kê các tag hiện có | Thỉnh thoảng |
| `git tag -a v1.0 -m "message"` | Tạo tag đánh dấu phiên bản release | Thỉnh thoảng |
| `git push --tags` | Đẩy toàn bộ tag lên remote | Thỉnh thoảng |

---

## 12. Quy trình làm việc nhóm đề xuất (Feature Branch Workflow)

Chuỗi lệnh tiêu chuẩn khi bắt đầu làm một tính năng mới trong dự án thực tế:

1. `git checkout dev && git pull` (Chuyển về nhánh dev và kéo code mới nhất về)
2. `git checkout -b feature/ten-tinh-nang` (Tạo nhánh tính năng mới)
3. *... Tiến hành chỉnh sửa code ...*
4. `git add . && git commit -m "Mô tả thay đổi"` (Lưu code vào commit)
5. `git pull --rebase origin main` (Đồng bộ code mới từ main về, tránh xung đột)
6. `git push -u origin feature/ten-tinh-nang` (Đẩy nhánh lên remote)
7. Tạo **Pull Request / Merge Request** trên GitHub, GitLab, Bitbucket...
8. Sau khi được review & approve $\rightarrow$ **Merge vào main**
9. `git branch -d feature/ten-tinh-nang` (Dọn dẹp xóa nhánh tính năng sau khi merge)

---

## 13. Top các lệnh dùng nhiều nhất khi làm việc nhóm

| STT | Lệnh |
|:---:|:---|
| 1 | `git status` |
| 2 | `git pull` |
| 3 | `git pull --rebase` |
| 4 | `git add .` |
| 5 | `git commit -m "..."` |
| 6 | `git push` |
| 7 | `git checkout -b <branch>` |
| 8 | `git merge <branch>` |
| 9 | `git log --oneline --graph --all` |
| 10 | `git stash / git stash pop` |

---

> ⚠️ **Lưu ý:** Các lệnh có mức độ **"Hiếm / cẩn trọng"** (`git reset --hard`, `git push --force`, `git branch -D`) có thể làm mất dữ liệu hoặc ghi đè lịch sử của người khác. Chỉ nên dùng khi hiểu rõ hậu quả và tuyệt đối tránh dùng trên nhánh chung (`main`/`develop`) nơi người khác đang làm việc.