# Writeup: [Tên bài Lab trên PortSwigger]

- **Chủ đề (Topic):** [VD: SQL Injection / XSS / SSRF...]
- **Cấp độ (Difficulty):** 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert
- **Lab URL:** `https://portswigger.net/web-security/...`
- **Mục tiêu (Goal):** [Mục tiêu cần đạt được để Solve bài lab, ví dụ: Lấy password user administrator, trigger alert(1), đọc file /etc/passwd...]

---

## 1. 🔍 Phân tích ban đầu (Recon & Vulnerability Analysis)

- **Điểm xuất hiện lỗ hổng (Vulnerable Parameter / Endpoint):**
  - Endpoint: `GET /filter?category=Gifts`
  - Parameter: `category`
- **Cơ chế hoạt động:**
  - [Mô tả ứng dụng xử lý input đầu vào như thế nào]
- **Nhận định loại lỗ hổng:**
  - [VD: SQLi Union-based, Blind SQLi with Conditional Errors, Stored XSS...]

---

## 2. 💣 Các bước khai thác chi tiết (Exploitation Steps)

### Bước 1: Thử nghiệm nhận biết phản hồi (Detection / Fuzzing)
- Thử gửi payload: `'` hoặc `' OR 1=1--`
- Quan sát kết quả: [Mô tả response hoặc lỗi 500/200]

### Bước 2: Xác định cấu trúc / Bypassing
- Xác định số cột / database type / filter rules:
  ```sql
  ' UNION SELECT NULL, NULL--
  ```

### Bước 3: Khai thác lấy dữ liệu / Đạt mục tiêu (Payload Execution)
- Payload hoàn chỉnh:
  ```sql
  ' UNION SELECT username, password FROM users--
  ```
- Kết quả thu được:
  - Username: `administrator`
  - Password: `...`

---

## 3. 🐍 Exploit Script (Tùy chọn nếu dùng Script Python)

```python
import requests
import sys
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}

def solve_lab(url):
    print("[+] Bắt đầu khai thác...")
    # Code exploit logic ở đây
    pass

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print(f"Usage: {sys.argv[0]} <URL>")
        sys.exit(1)
    solve_lab(sys.argv[1])
```

---

## 4. 📝 Bài học rút ra & Biện pháp phòng chống (Takeaways & Prevention)

- **Root Cause:** Do ứng dụng nối chuỗi SQL trực tiếp mà không dùng Parameterized Queries.
- **Biện pháp khắc phục (Remediation):**
  - Sử dụng Prepared Statements / Parameterized Queries.
  - Áp dụng nguyên tắc đặc quyền tối thiểu cho database user.
