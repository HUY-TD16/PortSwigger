# 🛡️ PortSwigger Web Security Academy - Notes & Writeups

> Kho lưu trữ tài liệu học tập, ghi chép lý thuyết, payload cheat sheets, lab writeups và công cụ tự động hóa giải lab trên [PortSwigger Web Security Academy](https://portswigger.net/web-security).

---

## 📂 Cấu trúc Repository

```text
PortSwigger/
├── README.md                      # Giới thiệu & Bảng theo dõi tiến độ tổng thể
├── templates/
│   └── writeup-template.md        # Mẫu ghi chép writeup chuẩn khi giải lab
├── sqli/                          # SQL Injection
│   ├── notes.md                   # Lý thuyết, phương pháp tìm kiếm & payloads
│   └── writeups/                  # Danh sách bài giải lab chi tiết
├── xss/                           # Cross-Site Scripting (Reflected, Stored, DOM)
│   ├── notes.md
│   └── writeups/
├── csrf/                          # Cross-Site Request Forgery
│   ├── notes.md
│   └── writeups/
├── ssrf/                          # Server-Side Request Forgery
│   ├── notes.md
│   └── writeups/
├── cors/                          # Cross-Origin Resource Sharing
│   ├── notes.md
│   └── writeups/
├── xxe/                           # XML External Entity Injection
│   ├── notes.md
│   └── writeups/
├── command-injection/             # OS Command Injection
│   ├── notes.md
│   └── writeups/
├── path-traversal/                # Directory / Path Traversal
│   ├── notes.md
│   └── writeups/
├── access-control/                # Broken Access Control & IDOR
│   ├── notes.md
│   └── writeups/
├── authentication/                # Authentication Vulnerabilities
│   ├── notes.md
│   └── writeups/
├── file-upload/                   # File Upload Vulnerabilities
│   ├── notes.md
│   └── writeups/
├── business-logic/                # Business Logic Vulnerabilities
│   ├── notes.md
│   └── writeups/
├── ssti/                          # Server-Side Template Injection
│   ├── notes.md
│   └── writeups/
├── request-smuggling/             # HTTP Request Smuggling (CL.TE, TE.CL, H2)
│   ├── notes.md
│   └── writeups/
├── jwt/                           # JSON Web Token Attacks
│   ├── notes.md
│   └── writeups/
├── oauth/                         # OAuth 2.0 Authentication Vulnerabilities
│   ├── notes.md
│   └── writeups/
├── race-conditions/               # Race Conditions (Limit overrun, Multi-step)
│   ├── notes.md
│   └── writeups/
├── prototype-pollution/           # Prototype Pollution (Client-side & Server-side)
│   ├── notes.md
│   └── writeups/
├── graphql/                       # GraphQL API Vulnerabilities
│   ├── notes.md
│   └── writeups/
├── nosql-injection/               # NoSQL Injection
│   ├── notes.md
│   └── writeups/
├── web-sockets/                   # WebSockets Security
│   ├── notes.md
│   └── writeups/
├── api-testing/                   # API Testing & Vulnerabilities
│   ├── notes.md
│   └── writeups/
└── tools/                         # Custom Python Exploit Scripts & Automation
    ├── README.md
    └── exploit_template.py        # Template script Python hỗ trợ proxy qua Burp Suite
```

---

## 📊 Bảng tiến độ học tập (Progress Tracker)

> Đánh dấu `[x]` khi hoàn thành các mục hoặc lab tương ứng.

| STT | Chủ đề (Topic) | Cấp độ | Ghi chép (Notes) | Số Lab đã giải | Trạng thái |
|:---:|:---|:---:|:---:|:---:|:---:|
| 01 | **SQL Injection (SQLi)** | 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert | [x] | 0 / 18 | 🔄 Đang học |
| 02 | **Cross-Site Scripting (XSS)** | 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 30 | ⏳ Chưa bắt đầu |
| 03 | **Cross-Site Request Forgery (CSRF)** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 12 | ⏳ Chưa bắt đầu |
| 04 | **Clickjacking** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 5 | ⏳ Chưa bắt đầu |
| 05 | **Cross-Origin Resource Sharing (CORS)** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 4 | ⏳ Chưa bắt đầu |
| 06 | **XML External Entity (XXE)** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 9 | ⏳ Chưa bắt đầu |
| 07 | **Server-Side Request Forgery (SSRF)** | 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 7 | ⏳ Chưa bắt đầu |
| 08 | **OS Command Injection** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 5 | ⏳ Chưa bắt đầu |
| 09 | **Path / Directory Traversal** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 6 | ⏳ Chưa bắt đầu |
| 10 | **Access Control & IDOR** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 13 | ⏳ Chưa bắt đầu |
| 11 | **Authentication Vulnerabilities** | 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 14 | ⏳ Chưa bắt đầu |
| 12 | **File Upload Vulnerabilities** | 🟢 Apprentice / 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 7 | ⏳ Chưa bắt đầu |
| 13 | **Server-Side Template Injection (SSTI)** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 7 | ⏳ Chưa bắt đầu |
| 14 | **Business Logic Vulnerabilities** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 11 | ⏳ Chưa bắt đầu |
| 15 | **HTTP Request Smuggling** | 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 22 | ⏳ Chưa bắt đầu |
| 16 | **JWT Attacks** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 8 | ⏳ Chưa bắt đầu |
| 17 | **OAuth 2.0 Authentication** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 6 | ⏳ Chưa bắt đầu |
| 18 | **Race Conditions** | 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 6 | ⏳ Chưa bắt đầu |
| 19 | **Prototype Pollution** | 🟡 Practitioner / 🔴 Expert | [ ] | 0 / 10 | ⏳ Chưa bắt đầu |
| 20 | **GraphQL API Vulnerabilities** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 5 | ⏳ Chưa bắt đầu |
| 21 | **NoSQL Injection** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 4 | ⏳ Chưa bắt đầu |
| 22 | **WebSockets** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 3 | ⏳ Chưa bắt đầu |
| 23 | **API Testing** | 🟢 Apprentice / 🟡 Practitioner | [ ] | 0 / 5 | ⏳ Chưa bắt đầu |

---

## 🎯 Quy chuẩn đặt tên & Viết Writeup

1. **Ghi chép lý thuyết (`notes.md`)**:
   - Tóm tắt bản chất lỗ hổng bằng ngôn ngữ cá nhân.
   - Các vector tấn công, dấu hiệu nhận biết, filter bypass techniques.
   - Cheat sheet payload hay dùng.
   - Cách phòng ngừa (Remediation).

2. **Bài giải lab (`writeups/<lab-name>.md`)**:
   - Sử dụng mẫu `templates/writeup-template.md`.
   - Nêu rõ: Mục tiêu bài lab, phân tích lỗ hổng, các bước khai thác chi tiết (từng bước trong Burp Suite / Python script) và kết quả (Screenshot / Flag).

3. **Script tự động (`tools/`)**:
   - Viết script Python rõ ràng, có comments, hỗ trợ proxy qua Burp Suite (`http://127.0.0.1:8080`) để debug.

---

## 🛠️ Công cụ thường dùng

- **Proxy & Scanner:** [Burp Suite Professional / Community](https://portswigger.net/burp)
- **Extensions:** Turbo Intruder, Hackvertor, Autorize, Logger++, DOM Invader, HTTP Request Smuggler.
- **Python Libraries:** `requests`, `pwntools`, `beautifulsoup4`, `urllib3`.
- **Wordlists:** [SecLists](https://github.com/danielmiessler/SecLists), PayloadAllTheThings.
