# 🛠️ Custom Exploit Scripts & Automation Tools

Thư mục này chứa các Python script tự viết để tự động hóa khai thác các bài lab trên PortSwigger Web Security Academy (hoặc các bài CTF / Pentest).

## 📁 Danh sách tệp & công cụ

- `exploit_template.py`: Template Python chuẩn tích hợp cấu hình proxy qua Burp Suite (`http://127.0.0.1:8080`), xử lý session và SSL warning.

## 🚀 Hướng dẫn sử dụng

### 1. Cài đặt thư viện cần thiết
```bash
pip install requests urllib3 beautifulsoup4 pwntools
```

### 2. Sử dụng Template giải lab
Chạy script truyền URL của bài lab:
```bash
python exploit_template.py https://<lab-id>.web-security-academy.net
```
*(Bật Burp Suite ở chế độ mặc định cổng 8080 để theo dõi chi tiết các request được gửi đi trong tab HTTP History).*
