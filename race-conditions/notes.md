# 🏎️ Race Conditions

## 1. 📌 Tổng quan
Race Condition xảy ra khi nhiều tiến trình hoặc luồng thực thi đồng thời truy cập và thao tác trên cùng một tài nguyên chia sẻ mà không có cơ chế khóa (Locking) phù hợp, dẫn đến xung đột trạng thái (Time-of-Check to Time-of-Use - TOCTOU).

---

## 2. 💣 Kỹ thuật khai thác với Burp Suite (Single-Packet Attack)

- **Limit Overrun:** Sử dụng nhiều lần mã giảm giá, rút tiền nhiều lần vượt quá số dư tài khoản.
- **Multi-step Race Conditions:** Can thiệp vào giữa các bước trong quy trình nhiều bước (ví dụ: kích hoạt tài khoản bằng token trước khi token bị hủy).
- **Công cụ:**
  - Sử dụng tính năng **Send group in parallel (last-byte sync)** trong tab Repeater của Burp Suite hoặc extension **Turbo Intruder** (sử dụng engine `Engine.BURP2`).

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng Database Locks (Pessimistic Locking / Optimistic Locking) hoặc Atomic Operations.
2. Thiết lập cơ chế Transaction Isolation Level phù hợp (Serializable).
