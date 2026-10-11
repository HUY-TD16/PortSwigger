# 🧠 Business Logic Vulnerabilities

## 1. 📌 Tổng quan
Lỗ hổng logic nghiệp vụ xảy ra do những giả định sai lầm trong thiết kế luồng xử lý của ứng dụng, cho phép kẻ tấn công điều khiển ứng dụng theo cách mà lập trình viên không lường trước (ví dụ: mua hàng với giá 0 đồng, tái sử dụng mã giảm giá, bỏ qua bước thanh toán).

---

## 2. 💣 Các dạng lỗ hổng logic điển hình

1. **Tin tưởng dữ liệu Client-side:**
   - Client gửi giá trị tiền (`price=100`) hoặc số lượng (`quantity=-1` / `quantity=0`) lên server mà server không kiểm tra lại từ database.
2. **Integer Overflow / Truncation:**
   - Nhập số lượng cực lớn khiến số tiền bị tràn số nguyên thành giá trị âm hoặc nhỏ.
3. **Flawed Multi-step Workflows:**
   - Nhảy cóc trực tiếp đến bước hoàn tất đơn hàng mà không qua bước thanh toán.
4. **Coupon / Voucher Reuse:**
   - Sử dụng lặp lại nhiều lần cùng một mã giảm giá do server không đánh dấu mã đã được dùng.
5. **Inconsistent State Handling:**
   - Rút tiền đồng thời trên nhiều luồng hoặc phiên đăng nhập trước khi số dư tài khoản được trừ.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Không bao giờ tin tưởng dữ liệu gửi từ client (luôn validate giá tiền, số lượng ở server).
2. Xây dựng State Machine chặt chẽ cho các quy trình nhiều bước (multi-step flows).
3. Đảm bảo tính toàn vẹn của giao dịch bằng cơ chế Transaction và Locks trong Database.
