# 📡 Server-Side Request Forgery (SSRF)

## 1. 📌 Tổng quan
SSRF là lỗ hổng bảo mật cho phép kẻ tấn công điều khiển server ứng dụng gửi các HTTP/Network request đến các dịch vụ nội bộ (Internal Network) hoặc các máy chủ bên ngoài không mong muốn.

---

## 2. 💣 Kỹ thuật khai thác & Bypass Filters

### Mục tiêu tấn công thường gặp:
- **Cloud Metadata APIs:**
  - AWS / GCP / Azure: `http://169.254.169.254/latest/meta-data/`
  - DigitalOcean: `http://169.254.169.254/metadata/v1.json`
- **Dịch vụ nội bộ:** `http://localhost/admin` hoặc `http://127.0.0.1/admin`
- **Internal Backend Systems:** Quét dải IP mạng nội bộ như `http://192.168.0.X/admin`

### Kỹ thuật Bypass Blacklist (127.0.0.1 / localhost):
- **Địa chỉ IP thay thế:**
  - Decimal: `http://2130706433/`
  - Hex: `http://0x7f000001/`
  - Octal: `http://017700000001/`
  - Shortened: `http://127.1` hoặc `http://0/`
  - IPv6: `http://[::1]/`
- **DNS Rebinding:** Trỏ một domain riêng tới IP nội bộ (VD: `127.0.0.1.nip.io`).
- **Bypass Whitelist bằng Open Redirect:** Lợi dụng endpoint Open Redirect để chuyển hướng request sang máy chủ nội bộ.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Network Layer Isolation:** Sử dụng Firewall / Network Segmentation để chặn truy cập trực tiếp từ Web Server vào Cloud Metadata và Internal Admin services.
2. **Strict URL Whitelisting:** Chỉ cho phép request đến danh sách domain/IP cụ thể, xác thực sau khi phân giải DNS để chống DNS Rebinding.
