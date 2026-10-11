# 🔑 Authentication Vulnerabilities

## 1. 📌 Tổng quan
Lỗ hổng xác thực xảy ra khi các cơ chế nhận dạng và xác minh danh tính người dùng bị thiết kế hoặc triển khai lỗi, cho phép kẻ tấn công mạo danh người dùng hợp pháp hoặc chiếm quyền tài khoản (Account Takeover).

---

## 2. 💣 Các lỗ hổng xác thực phổ biến

1. **Username Enumeration:**
   - Dựa vào thông báo lỗi khác nhau (VD: "Invalid password" vs "User does not exist").
   - Dựa vào chênh lệch thời gian phản hồi (Response time analysis).
2. **Brute-force / Password Spraying:**
   - Thiếu cơ chế Rate Limiting hoặc Lockout tài khoản.
   - Bypass Rate Limit bằng cách đổi IP (Header `X-Forwarded-For`), chèn ký tự đặc biệt hoặc dùng IP rotation.
3. **2FA / MFA Bypass:**
   - Bỏ qua bước xác minh 2FA (gọi trực tiếp endpoint sau 2FA).
   - Đoán mã 2FA (OTP 4 số hoặc 6 số) do không giới hạn số lần thử.
   - Brute-force mã 2FA đồng thời với nhiều session.
4. **Password Reset Poisoning:**
   - Can thiệp header `Host` hoặc `X-Forwarded-Host` trong request quên mật khẩu để hướng link reset về server của attacker.
5. **Remember Me Token Flaws:**
   - Cookie Remember Me được tạo bằng cách mã hóa yếu (Base64 của `username:md5(password)`).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng thông báo lỗi chung chung (VD: "Invalid username or password").
2. Triển khai Rate Limiting và CAPTCHA hiệu quả trên các endpoint đăng nhập và reset mật khẩu.
3. Bắt buộc sử dụng 2FA mạnh mẽ và hủy toàn bộ phiên làm việc cũ khi đổi mật khẩu.
