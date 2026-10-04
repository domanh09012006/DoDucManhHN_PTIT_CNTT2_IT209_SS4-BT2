# BÁO CÁO THỰC HÀNH: BÀI 2
## Quản lý nhánh và Giải quyết xung đột (Merge Conflict)

- **Đường dẫn nộp bài:** `homework/session_04/ex2/`
- **Học viên thực hiện:** DoDucManh
- **Email:** doducmanh09012006@gmail.com
- **Môi trường thực thi:** PowerShell / Git Bash

---

## 1. Mục tiêu bài tập

1. Tạo và chuyển đổi linh hoạt giữa nhánh chính `main` và nhánh tính năng `feature-update`.
2. Chủ động tạo ra xung đột mã nguồn (Merge Conflict) trên cùng một dòng ở tệp `README.md`.
3. Nhận diện và xử lý thủ công các ký hiệu đánh dấu xung đột (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Thực hiện gộp 3 vùng (3-Way Merge), hoàn tất Merge Commit và kiểm tra đồ thị phân nhánh bằng lệnh `git log --graph --oneline`.

---

## 2. Các bước tiến hành thực nghiệm

### Bước 1: Khởi tạo thư mục và commit gốc trên nhánh `main`

```powershell
# Chuyển vào thư mục bài tập 2
cd "D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2"

# Khởi tạo repository
git init -b main
git config --local user.name "DoDucManh"
git config --local user.email "doducmanh09012006@gmail.com"

# Tạo tệp README.md ban đầu
Set-Content README.md "# Dự án Quản lý Phiên bản"
Add-Content README.md "Phiên bản khởi tạo: v1.0.0"

# Thực hiện commit gốc (Base Commit)
git add README.md
git commit -m "docs: base commit on main"
```

---

### Bước 2: Tạo nhánh `feature-update` và chỉnh sửa

Tách nhánh tính năng mới từ nhánh `main` và sửa nội dung dòng thứ hai:

```powershell
git checkout -b feature-update

# Sửa nội dung dòng 2 thành tính năng mới
Set-Content README.md "# Dự án Quản lý Phiên bản"
Add-Content README.md "Phiên bản: v1.1.0 - Tính năng mới cập nhật trên branch feature-update"

git add README.md
git commit -m "feat: update version description on feature-update"
```

---

### Bước 3: Quay lại nhánh `main` và chỉnh sửa cùng dòng (Tạo xung đột)

Chuyển về nhánh `main` và thay đổi dòng thứ hai thành nội dung khác nhằm gây xung đột trực tiếp:

```powershell
git checkout main

# Sửa cùng vị trí dòng 2 trên nhánh main
Set-Content README.md "# Dự án Quản lý Phiên bản"
Add-Content README.md "Phiên bản: v1.0.1 - Bản vá khẩn cấp trực tiếp trên main"

git add README.md
git commit -m "fix: update version description on main"
```

---

### Bước 4: Thực hiện Merge và kích hoạt Merge Conflict

Thực hiện gộp nhánh `feature-update` vào `main`:

```powershell
git merge feature-update
```

**Thông báo xung đột xuất hiện:**
```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

---

### Bước 5: Phân tích và Xử lý xung đột thủ công

Mở tệp `README.md`, Git tự động chèn các đánh dấu xung đột:

```markdown
# Dự án Quản lý Phiên bản
<<<<<<< HEAD
Phiên bản: v1.0.1 - Bản vá khẩn cấp trực tiếp trên main
=======
Phiên bản: v1.1.0 - Tính năng mới cập nhật trên branch feature-update
>>>>>>> feature-update
```

#### Ý nghĩa các thẻ đánh dấu:
- `<<<<<<< HEAD`: Bắt đầu phần thay đổi ở nhánh hiện tại bạn đang đứng (`main`).
- `=======`: Vạch ngăn cách giữa thay đổi của 2 nhánh.
- `>>>>>>> feature-update`: Kết thúc phần thay đổi đến từ nhánh được gộp (`feature-update`).

#### Quyết định dung hòa nội dung:
Xóa toàn bộ các thẻ đánh dấu (`<<<<<<<`, `=======`, `>>>>>>>`) và gộp ý nghĩa của cả hai nhánh thành phiên bản thống nhất:

```markdown
# Dự án Quản lý Phiên bản
Phiên bản: v1.1.1 - Hợp nhất bản vá khẩn cấp và tính năng mới từ feature-update
```

---

### Bước 6: Đưa tệp đã sửa vào Staging và hoàn thành Merge Commit

```powershell
git add README.md
git commit -m "merge: resolve merge conflict between main and feature-update"
```

---

## 3. Minh chứng kết quả thực thi (Terminal Logs)

Dưới đây là toàn bộ log thao tác thực tế xác nhận xung đột và hoàn thành giải quyết:

```powershell
PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git merge feature-update
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.

PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" to track)

PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git add README.md

PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git commit -m "merge: resolve merge conflict between main and feature-update"
[main 9d42f1a] merge: resolve merge conflict between main and feature-update

PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git status
On branch main
nothing to commit, working tree clean
```

### Minh chứng Đồ thị phân nhánh (`git log --graph --oneline`)

```powershell
PS D:\Doanh nghiep\[IT-209] Devops Fundamentals\Session04\homework\ex2> git log --graph --oneline
*   9d42f1a (HEAD -> main) merge: resolve merge conflict between main and feature-update
|\  
| * e4a112c (feature-update) feat: update version description on feature-update
* | 3c812bb fix: update version description on main
|/  
* 81b0a9f docs: base commit on main
```

---

## 4. Giải thích cơ chế 3-Way Merge

Quá trình gộp nhánh xảy ra xung đột khi có sự khác biệt giữa 3 commit:
1. **Base Commit (`81b0a9f`)**: Tổ tiên chung gần nhất (Common Ancestor) của cả 2 nhánh.
2. **Current Branch Commit (`3c812bb` - HEAD/main)**: Dòng 2 được sửa thành bản vá lỗi.
3. **Incoming Branch Commit (`e4a112c` - feature-update)**: Cùng dòng 2 đó được sửa thành tính năng mới.

Vì cả hai nhánh đều thay đổi cùng một vị trí tính từ Base Commit nên Git không thể tự phán đoán phiên bản nào đúng. Lập trình viên phải can thiệp thủ công để chọn lọc, hợp nhất nội dung và xác nhận bằng một commit gộp (Merge Commit `9d42f1a`) có 2 commit cha (Parents).