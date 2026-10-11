# 🔑 OAuth 2.0 Authentication Vulnerabilities

## 1. 📌 Tổng quan
OAuth 2.0 là framework ủy quyền cho phép các ứng dụng bên thứ ba truy cập tài nguyên của người dùng mà không cần biết mật khẩu của họ. Các lỗ hổng OAuth thường dẫn đến việc chiếm đoạt tài khoản (Account Takeover) hoặc đánh cắp Authorization Code/Access Token.

---

## 2. 💣 Các lỗ hổng OAuth phổ biến

1. **Flawed CSRF Protection / Missing `state` Parameter:**
   - Không kiểm tra tham số `state` trong OAuth flow, cho phép kẻ tấn công liên kết tài khoản mạng xã hội của chúng với tài khoản nạn nhân.
2. **Redirect URI Flaws (Stealing Codes/Tokens):**
   - Không xác thực nghiêm ngặt tham số `redirect_uri` (cho phép subdomain takeover, path traversal, open redirect để gửi code về server attacker).
3. **Flawed Scope Validation:**
   - Ứng dụng cấp quyền vượt quá mức cần thiết hoặc không kiểm tra lại token scope khi gọi resource API.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Bắt buộc sử dụng tham số `state` có tính chất ngẫu nhiên, gắn với session người dùng để chống CSRF.
2. Thiết lập Whitelist chặt chẽ cho `redirect_uri` (khớp hoàn toàn cả domain, port và path).
3. Sử dụng PKCE (Proof Key for Code Exchange) ngay cả trên Web Applications.
