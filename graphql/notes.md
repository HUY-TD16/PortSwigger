# 📊 GraphQL API Vulnerabilities

## 1. 📌 Tổng quan
GraphQL là ngôn ngữ truy vấn dành cho API, cho phép client yêu cầu chính xác dữ liệu họ cần. Các lỗ hổng bảo mật thường phát sinh do bật tính năng dò tìm lược đồ (Introspection), thiếu phân quyền đối tượng hoặc tấn công từ chối dịch vụ (DoS qua Nested Queries).

---

## 2. 💣 Kỹ thuật khai thác

### 1. Introspection Query (Dò tìm Schema):
Truy vấn toàn bộ cấu trúc Schema, types, queries và mutations:
```graphql
query {
  __schema {
    types {
      name
      fields {
        name
      }
    }
  }
}
```

### 2. Bypass Introspection Filter:
- Thử chèn ký tự đặc biệt hoặc newline trước `__schema`:
  ```graphql
  query {
    " \n\r __schema" { ... }
  }
  ```

### 3. Khai thác Field Suggestions:
- Khi gõ sai tên field, GraphQL trả về gợi ý: "Did you mean 'password'?" -> Thu thập danh sách field bí mật.

### 4. Bypassing Rate Limits (Batching Attacks):
- Gửi nhiều mutations hoặc queries trong cùng 1 HTTP request để brute-force password / OTP mà không bị trigger rate limit.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Tắt chế độ Introspection trên môi trường Production.
2. Thiết lập giới hạn độ sâu truy vấn (Query Depth Limiting) và giới hạn chi phí truy vấn (Query Cost Analysis).
3. Kiểm tra quyền truy cập (Authorization) ở mức từng Field Resolver.
