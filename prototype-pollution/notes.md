# 🧬 Prototype Pollution

## 1. 📌 Tổng quan
Prototype Pollution là lỗ hổng trong JavaScript cho phép kẻ tấn công ghi đè hoặc bổ sung thuộc tính tùy ý vào `Object.prototype`, từ đó ảnh hưởng đến tất cả các đối tượng (objects) kế thừa trong ứng dụng, có thể dẫn đến DOM XSS (Client-side) hoặc RCE / Bypass Auth (Server-side NodeJS).

---

## 2. 💣 Kỹ thuật khai thác

### Client-side Prototype Pollution:
- **Sources phổ biến:** URL Query params, URL Hash, JSON parsing từ `postMessage` hoặc WebSockets.
- **Paylod thử nghiệm:**
  ```text
  ?__proto__[testproperty]=antigravity
  ?__proto__.testproperty=antigravity
  ?constructor.prototype.testproperty=antigravity
  ```
- **Gadgets:** Tìm các đoạn code client-side sử dụng thuộc tính chưa được khởi tạo để chuyển hóa thành DOM XSS.

### Server-side Prototype Pollution (NodeJS):
- Khai thác qua các hàm merge/extend đối tượng không an toàn (VD: `lodash.merge`).
- RCE Gadget qua `child_process.fork()` / `child_process.execFile()` bằng cách inject `shell`, `execArgv` hoặc `NODE_OPTIONS`.

---

## 3. 🛡️ Biện pháp phòng chống (Mitigation)
1. Sử dụng `Object.create(null)` để tạo object không kế thừa prototype.
2. Đóng băng prototype bằng `Object.freeze(Object.prototype)`.
3. Kiểm tra và lọc các khóa nguy hiểm (`__proto__`, `constructor`, `prototype`) trước khi thực hiện merge object.
