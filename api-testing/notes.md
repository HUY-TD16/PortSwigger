# 🔌 API Testing & Vulnerabilities

## 1. 📌 Tổng quan
API Testing tập trung vào việc tìm kiếm các lỗ hổng bảo mật trong các giao diện lập trình ứng dụng (RESTful, SOAP, GraphQL...). Các lỗ hổng phổ biến bao gồm rò rỉ tài liệu API (API Documentation), thiếu kiểm tra phân quyền (BOLA / BFLA), Mass Assignment, và thiếu giới hạn tần suất (Rate Limiting).

---

## 2. 💣 Kỹ thuật kiểm thử & Khai thác

### 1. Dò tìm tài liệu API (API Discovery):
Tìm kiếm các endpoint chứa tài liệu hoặc schema Swagger/OpenAPI:
- `/api`, `/api-docs`, `/swagger.json`, `/openapi.json`, `/v1/api-docs`, `/v2/api-docs`

### 2. Thay đổi HTTP Method:
- Nếu `GET /api/users/123` bị chặn, thử đổi sang `POST`, `PUT`, `PATCH`, `DELETE` hoặc dùng header override:
  - `X-HTTP-Method-Override: PUT`

### 3. Mass Assignment:
- Gửi thêm các thuộc tính nhạy cảm trong JSON body mà không được hiển thị trên form UI:
  ```json
  {
    "username": "user1",
    "email": "user1@mail.com",
    "isAdmin": true,
    "role": "admin"
  }
  ```

### 4. BOLA (Broken Object Level Authorization):
- Thay đổi ID người dùng/đơn hàng trong URL API (`/api/v1/orders/1001` -> `/api/v1/orders/1002`).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Ẩn hoặc phân quyền truy cập vào tài liệu API (Swagger UI / OpenAPI schema).
2. Sử dụng DTO (Data Transfer Objects) hoặc Whitelist thuộc tính để ngăn chặn Mass Assignment.
3. Luôn xác thực quyền sở hữu dữ liệu trên từng API request.
