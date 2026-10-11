# 🚪 Broken Access Control & IDOR

## 1. 📌 Tổng quan
Broken Access Control xảy ra khi ứng dụng không thực thi đúng quy tắc phân quyền người dùng, dẫn đến việc người dùng có thể truy cập các tài nguyên hoặc chức năng ngoài phạm vi cho phép.

### Các dạng phổ biến:
1. **Vertical Privilege Escalation:** Người dùng thông thường truy cập trái phép chức năng quản trị viên (Admin).
2. **Horizontal Privilege Escalation (IDOR - Insecure Direct Object References):** Người dùng A truy cập dữ liệu của người dùng B (ví dụ đổi `id=123` thành `id=124`).
3. **Multi-step Process Flaws:** Bỏ qua các bước kiểm tra quyền ở bước đầu để gọi trực tiếp API ở bước cuối.
4. **Context-dependent Privilege Escalation:** Chuyển đổi ngữ cảnh (ví dụ từ tài khoản Buyer sang Seller).

---

## 2. 🔍 Kỹ thuật nhận diện & Khai thác

- **Unprotected Admin Endpoints:** Kiểm tra `robots.txt`, file JavaScript client-side hoặc đoán URL (`/admin`, `/administrator`, `/api/admin`).
- **Parameter-based Access Control:** Can thiệp tham số `isAdmin=true`, `role=admin`, `roleid=1` trong request / cookie / JSON body.
- **Broken Access Control do HTTP Method:** Endpoint chặn `GET /admin` nhưng lại cho phép `POST /admin` hoặc ngược lại, hoặc bypass qua header `X-Original-URL: /admin` / `X-Rewrite-URL: /admin`.
- **IDOR trong API:** Thay đổi ID trong URL endpoint `/api/users/{id}/profile`, `/api/invoices/{id}` hoặc GUID dự đoán được.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Deny by Default:** Mặc định từ chối tất cả truy cập ngoại trừ tài nguyên công khai (Public resources).
2. **Server-side Authorization Checks:** Kiểm tra quyền sở hữu đối tượng ở phía máy chủ trên mọi API endpoint.
3. **Sử dụng Burp Extension:** Cài đặt extension **Autorize** để kiểm thử phân quyền tự động.
