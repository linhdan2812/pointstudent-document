# Test Cases – POST /auth/login

> **FR-001** – Đăng nhập hệ thống
> **Endpoint:** `POST /auth/login`
> **Ngày tạo:** 2026-03-07

---

## Positive Tests

| ID | Mô tả mục đích test | Method | Endpoint | Request Body | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|---|---|
| TC-LOGIN-001 | Đăng nhập thành công với tài khoản **teacher** hợp lệ | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "Teacher123@" }` | HTTP 200; `success: true`; `message: "Đăng nhập thành công"`; có `accessToken` (JWT); `user.role = "teacher"`; `user.school_id` khác null | |
| TC-LOGIN-002 | Đăng nhập thành công với tài khoản **admin_system** hợp lệ | POST | `/auth/login` | `{ "email": "adminstpoint@gmail.com", "password": "adminstpoiNt1122@" }` | HTTP 200; `success: true`; `user.role = "admin_system"`; `user.school_id = null` (BR-001-01) | |
| TC-LOGIN-003 | Đăng nhập thành công với tài khoản **admin_school** hợp lệ | POST | `/auth/login` | `{ "email": "admin.school@test.com", "password": "AdminSchool123@" }` | HTTP 200; `success: true`; `user.role = "admin_school"`; `user.school_id` khác null | |
| TC-LOGIN-004 | Đăng nhập thành công với tài khoản **parent** hợp lệ | POST | `/auth/login` | `{ "email": "parent.c@test.com", "password": "Parent123@" }` | HTTP 200; `success: true`; `user.role = "parent"`; `user.school_id` khác null | |
| TC-LOGIN-005 | Đăng nhập thành công với tài khoản **student** hợp lệ | POST | `/auth/login` | `{ "email": "linhdanzyl2812@gmail.com", "password": "Student123@" }` | HTTP 200; `success: true`; `user.role = "student"` | |
| TC-LOGIN-006 | Xác minh cấu trúc response trả về đầy đủ các field | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "Teacher123@" }` | Response body có đủ: `success`, `message`, `data.accessToken`, `data.user.id`, `data.user.email`, `data.user.role`, `data.user.school_id` | |


---

## Negative Tests – Sai thông tin đăng nhập

| ID | Mô tả mục đích test | Method | Endpoint | Request Body | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|---|---|
| TC-LOGIN-007 | Đăng nhập với **email không tồn tại** trong hệ thống | POST | `/auth/login` | `{ "email": "notexist@school.edu.vn", "password": "secret123" }` | HTTP 401; `success: false`; `message: "Email hoặc mật khẩu không đúng"` | |
| TC-LOGIN-008 | Đăng nhập với **mật khẩu sai** (email đúng) | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "wrongpass" }` | HTTP 401; `success: false`; `message: "Email hoặc mật khẩu không đúng"` | |
| TC-LOGIN-009 | Xác minh thông báo lỗi **giống nhau** cho cả 2 trường hợp: email sai & mật khẩu sai (tránh lộ thông tin) | POST | `/auth/login` | Case A: `{ "email": "notexist@test.com", "password": "Teacher123@" }` / Case B: `{ "email": "teacher.a@test.com", "password": "wrongpass" }` | Cả 2 trường hợp đều trả về HTTP 401 với cùng `message: "Email hoặc mật khẩu không đúng"` | |
| TC-LOGIN-010 | Đăng nhập với **mật khẩu đúng nhưng email viết hoa** (kiểm tra case-sensitive email) | POST | `/auth/login` | `{ "email": "TEACHER.A@TEST.COM", "password": "Teacher123@" }` | HTTP 401 hoặc HTTP 200 (tùy thiết kế) – ghi nhận hành vi thực tế | |
| TC-LOGIN-011A | Đăng nhập với **mật khẩu có khoảng trắng thừa** ở đầu | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": " Teacher123@ " }` | HTTP 401; password phải khớp chính xác | |
| TC-LOGIN-011B | Đăng nhập với **mật khẩu có khoảng trắng thừa** ở cuỗi | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": " Teacher123@ " }` | HTTP 401; password phải khớp chính xác | |
---

## Negative Tests – Validation (Thiếu / Rỗng / Sai định dạng)

| ID | Mô tả mục đích test | Method | Endpoint | Request Body | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|---|---|
| TC-LOGIN-012 | Gửi request **không có field `email`** | POST | `/auth/login` | `{ "password": "secret123" }` | HTTP 422; `errors.email: ["Vui lòng nhập email"]` | |
| TC-LOGIN-013 | Gửi request **không có field `password`** | POST | `/auth/login` | `{ "email": "teacher.a@test.com" }` | HTTP 422; `errors.password: ["Vui lòng nhập mật khẩu"]` | |
| TC-LOGIN-014 | Gửi request **body rỗng** `{}` | POST | `/auth/login` | `{}` | HTTP 422; `errors.email` và `errors.password` đều có lỗi | |
| TC-LOGIN-015 | Gửi request **không có body** (body = null) | POST | `/auth/login` | _(không có body)_ | HTTP 422; trả về lỗi validation | |
| TC-LOGIN-016 | `email` là **chuỗi rỗng** `""` | POST | `/auth/login` | `{ "email": "", "password": "Teacher123@" }` | HTTP 422; `errors.email: ["Vui lòng nhập email"]` | |
| TC-LOGIN-017 | `password` là **chuỗi rỗng** `""` | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "" }` | HTTP 422; `errors.password: ["Vui lòng nhập mật khẩu"]` | |
| TC-LOGIN-018 | `email` **sai định dạng** – thiếu ký tự `@` | POST | `/auth/login` | `{ "email": "teacher.atest.com", "password": "Teacher123@" }` | HTTP 422; `errors.email: ["Vui lòng nhập đúng định dạng email"]` | |
| TC-LOGIN-019 | `email` **sai định dạng** – thiếu domain (ví dụ `teacher.a@`) | POST | `/auth/login` | `{ "email": "teacher.a@", "password": "Teacher123@" }` | HTTP 422; `errors.email: ["Vui lòng nhập đúng định dạng email"]` | |
| TC-LOGIN-020 | `email` **sai định dạng** – chỉ là khoảng trắng `"   "` | POST | `/auth/login` | `{ "email": "   ", "password": "Teacher123@" }` | HTTP 422; lỗi định dạng hoặc bắt buộc email | |
| TC-LOGIN-021 | `email` là **số nguyên** thay vì string | POST | `/auth/login` | `{ "email": 12345, "password": "Teacher123@" }` | HTTP 422; lỗi validation kiểu dữ liệu | |
| TC-LOGIN-022 | `password` là **số nguyên** thay vì string | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": 123456 }` | HTTP 422; lỗi validation kiểu dữ liệu | |
| TC-LOGIN-023 | Gửi thêm **field không hợp lệ** trong body (extra fields) | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "Teacher123@", "role": "admin_system" }` | HTTP 200 đăng nhập bình thường; field thừa bị bỏ qua | |

---

## Boundary Tests – Giá trị biên

| ID | Mô tả mục đích test | Method | Endpoint | Request Body | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|---|---|
| TC-LOGIN-024 | `email` hợp lệ có **độ dài tối đa** (254 ký tự – chuẩn RFC 5321) | POST | `/auth/login` | email 254 ký tự hợp lệ, password đúng | HTTP 200 nếu tồn tại; hoặc HTTP 401 nếu không tồn tại — không được trả về 422 | |
| TC-LOGIN-025 | `email` vượt quá **255 ký tự** | POST | `/auth/login` | email > 255 ký tự | HTTP 422; lỗi validation | |
| TC-LOGIN-026 | `password` có **1 ký tự** (kiểm tra giới hạn dưới nếu có) | POST | `/auth/login` | `{ "email": "teacher.a@test.com", "password": "a" }` | HTTP 401 (không khớp) hoặc HTTP 200 nếu hệ thống cho phép password 1 ký tự | |
| TC-LOGIN-027 | `password` rất dài **(1000 ký tự)** | POST | `/auth/login` | password = chuỗi 1000 ký tự | HTTP 401 (không khớp) hoặc HTTP 422 nếu có giới hạn max length | |

---

## Security / Edge Case Tests

| ID | Mô tả mục đích test | Method | Endpoint | Request Body | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|---|---|
| TC-LOGIN-028 | `email` chứa ký tự **SQL Injection** | POST | `/auth/login` | `{ "email": "' OR '1'='1", "password": "anything" }` | HTTP 422 hoặc HTTP 401; không được thực thi SQL injection | |
| TC-LOGIN-029 | `password` chứa ký tự **đặc biệt** hợp lệ (`!@#$%^&*`) | POST | `/auth/login` | `{ "email": "adminstpoint@gmail.com", "password": "adminstpoiNt1122@" }` | HTTP 200 nếu đúng mật khẩu; không bị lỗi parsing (dùng account admin_system có ký tự đặc biệt) | |
| TC-LOGIN-030 | Gọi API với **Content-Type không phải** `application/json` | POST | `/auth/login` | Form-urlencoded: `email=teacher.a@test.com&password=Teacher123@` | HTTP 422 hoặc 400; không crash server | |
| TC-LOGIN-031 | Gọi API bằng **phương thức GET** thay vì POST | GET | `/auth/login` | _(không có body)_ | HTTP 404 hoặc 405 Method Not Allowed | |

---

## Ghi chú

- Cột **Kết quả thực tế** điền sau khi chạy test thực tế.
- TC-LOGIN-002: cần có tài khoản `admin_system` trong DB để xác minh `user.school_id = null`.
- TC-LOGIN-009: test quan trọng về bảo mật — hai case phải cho ra **cùng một message lỗi**.
- TC-LOGIN-010, TC-LOGIN-026, TC-LOGIN-027: test khám phá hành vi — không có expected result cứng, cần ghi nhận thực tế.
