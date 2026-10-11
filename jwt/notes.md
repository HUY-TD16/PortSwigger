# 🎫 JWT (JSON Web Token) Attacks

## 1. 📌 Cấu trúc của JWT
JWT gồm 3 phần phân tách bằng dấu chấm `.`:
- **Header:** Chứa thuật toán ký (`alg`) và kiểu token (`typ`).
- **Payload:** Chứa các claims (thông tin user, quyền, thời gian hết hạn...).
- **Signature:** Chữ ký số xác minh tính toàn vẹn của token.

---

## 2. 💣 Kỹ thuật tấn công phổ biến

1. **Unverified Signature:** Server chỉ decode payload mà quên xác thực chữ ký (Signature).
2. **Algorithm None (`"alg": "none"`):** Đổi thuật toán ký thành `none`, xóa phần signature để gửi request với quyền tùy ý.
3. **Weak HMAC Secret Key (Brute-force):** Sử dụng Hashcat hoặc John The Ripper để crack secret key khi thuật toán là `HS256`.
   ```bash
   hashcat -a 0 -m 16500 jwt.txt /path/to/jwt.secrets.list
   ```
4. **Algorithm Confusion (RS256 sang HS256):** Đổi `RS256` (Asymmetric) sang `HS256` (Symmetric) và ký lại token bằng chính Public Key của server.
5. **JWK / JKU / KID Header Injection:**
   - Chèn public key tự tạo vào header `jwk`.
   - Giả mạo URL chứa key qua header `jku`.
   - Lợi dụng Path Traversal hoặc SQLi qua tham số `kid` (VD: `kid: ../../../dev/null` hoặc SQLi lấy secret).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Luôn xác thực chữ ký token trước khi đọc payload.
2. Không chấp nhận thuật toán `none`.
3. Sử dụng secret key phức tạp, khó đoán hoặc dùng cặp khóa Bất đối xứng (RS256/ES256).
