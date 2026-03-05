# API Documentation

> **Hệ thống Quản lý Điểm Học sinh**
> Base URL: `http://localhost:{PORT}/api`
> Content-Type: `application/json`

---

## Danh sách API

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/health` | Kiểm tra trạng thái server |
| `POST` | `/auth/login` | Đăng nhập, trả về JWT |
| `POST` | `/auth/forgot-password` | Gửi email link đặt lại mật khẩu |
| `POST` | `/auth/reset-password` | Đặt lại mật khẩu bằng token |
| `GET` | `/schools` | Lấy danh sách trường học (có tìm kiếm) |
| `POST` | `/schools` | Tạo trường học mới + tài khoản Admin School |
| `GET` | `/schools/:id` | Lấy chi tiết một trường học |
| `PUT` | `/schools/:id` | Cập nhật thông tin trường học |
| `DELETE` | `/schools/:id` | Xóa trường học (chỉ khi chưa có master data) |
| `GET` | `/academic-years` | Lấy danh sách năm học của trường |
| `POST` | `/academic-years` | Tạo năm học mới |
| `GET` | `/academic-years/:id` | Lấy chi tiết một năm học |
| `PUT` | `/academic-years/:id` | Cập nhật thông tin năm học (tên, ngày) |
| `DELETE` | `/academic-years/:id` | Xóa năm học (chỉ khi trạng thái `preparing`) |
| `GET` | `/subjects` | Lấy danh sách môn học theo năm học |
| `POST` | `/subjects` | Tạo môn học mới |
| `GET` | `/subjects/:id` | Lấy chi tiết một môn học |
| `PUT` | `/subjects/:id` | Cập nhật tên môn học |
| `DELETE` | `/subjects/:id` | Xóa môn học |
| `GET` | `/teachers` | Lấy danh sách giáo viên của trường (có tìm kiếm) |
| `POST` | `/teachers` | Tạo giáo viên mới + tài khoản đăng nhập |
| `GET` | `/teachers/:id` | Lấy chi tiết một giáo viên |
| `PUT` | `/teachers/:id` | Cập nhật thông tin giáo viên |
| `GET` | `/students` | Lấy danh sách học sinh của trường (có tìm kiếm, lọc) |
| `POST` | `/students` | Tạo học sinh mới + tài khoản học sinh & phụ huynh |
| `GET` | `/students/:id` | Lấy chi tiết một học sinh |
| `PUT` | `/students/:id` | Cập nhật thông tin học sinh |
| `GET` | `/classes` | Lấy danh sách lớp học (có lọc theo năm học, tìm kiếm) |
| `POST` | `/classes` | Tạo lớp học mới |
| `GET` | `/classes/:id` | Lấy chi tiết một lớp học |
| `PUT` | `/classes/:id` | Cập nhật tên lớp hoặc giáo viên chủ nhiệm |
| `GET` | `/classes/available-teachers` | Lấy danh sách giáo viên chưa là GVCN trong năm học |
| `GET` | `/classes/:id/students` | Lấy danh sách học sinh trong lớp |
| `GET` | `/classes/:id/available-students` | Lấy danh sách học sinh khả dụng để thêm vào lớp |
| `POST` | `/classes/:id/students` | Thêm học sinh vào lớp (batch) |
| `DELETE` | `/classes/:id/students/:student_id` | Xóa học sinh khỏi lớp |
| `GET` | `/classes/:id/assignments` | Lấy danh sách phân công giáo viên bộ môn của lớp |
| `POST` | `/classes/:id/assignments` | Tạo phân công mới (môn học + giáo viên) |
| `PUT` | `/classes/:id/assignments/:assignment_id` | Cập nhật giáo viên phụ trách môn học |
| `DELETE` | `/classes/:id/assignments/:assignment_id` | Xóa phân công |
| `GET` | `/teacher/dashboard` | Dashboard giáo viên: lớp chủ nhiệm + lớp bộ môn (năm học đang diễn ra) |
| `GET` | `/teacher/homeroom/:class_id/students` | Lấy danh sách học sinh lớp chủ nhiệm (kèm thông tin đầy đủ) |
| `GET` | `/teacher/homeroom/:class_id/score-overview` | Bảng điểm tổng hợp (GVCN view): ĐTB từng môn của từng HS |
| `GET` | `/teacher/homeroom/:class_id/students/:student_id/score-detail` | Bảng điểm chi tiết của một học sinh (GVCN view) |
| `GET` | `/teacher/homeroom/:class_id/students/:student_id/comments` | Lấy danh sách nhận xét của học sinh |
| `POST` | `/teacher/homeroom/:class_id/students/:student_id/comments` | Tạo nhận xét mới |
| `PUT` | `/teacher/homeroom/:class_id/comments/:comment_id` | Cập nhật nhận xét (chỉ khi còn "Đã lên lịch") |
| `GET` | `/teacher/assignments/:assignment_id/scores` | Bảng điểm chi tiết lớp bộ môn |
| `POST` | `/teacher/assignments/:assignment_id/score-columns` | Thêm cột điểm mới (chọn hệ số) |
| `PUT` | `/teacher/assignments/:assignment_id/scores` | Lưu điểm hàng loạt + tính lại ĐTB |
| `GET` | `/portal/parent` | Cổng phụ huynh: danh sách con liên kết với tài khoản |
| `GET` | `/portal/parent/students/:student_id` | Bảng điểm + nhận xét của con được chọn |
| `GET` | `/portal/student` | Cổng học sinh: bảng điểm + nhận xét cá nhân |

---

## Mục lục

- [Quy ước chung](#quy-ước-chung)
  - [Cấu trúc Response](#cấu-trúc-response)
  - [HTTP Status Codes](#http-status-codes)
  - [Roles](#roles)
- [Health Check](#health-check)
- [Auth](#auth)
  - [POST /auth/login](#post-authlogin)
  - [POST /auth/forgot-password](#post-authforgot-password)
  - [POST /auth/reset-password](#post-authreset-password)
- [Schools](#schools)
  - [GET /schools](#get-schools)
  - [POST /schools](#post-schools)
  - [GET /schools/:id](#get-schoolsid)
  - [PUT /schools/:id](#put-schoolsid)
  - [DELETE /schools/:id](#delete-schoolsid)
- [Academic Years](#academic-years)
  - [GET /academic-years](#get-academic-years)
  - [POST /academic-years](#post-academic-years)
  - [GET /academic-years/:id](#get-academic-yearsid)
  - [PUT /academic-years/:id](#put-academic-yearsid)
  - [DELETE /academic-years/:id](#delete-academic-yearsid)
- [Subjects](#subjects)
  - [GET /subjects](#get-subjects)
  - [POST /subjects](#post-subjects)
  - [GET /subjects/:id](#get-subjectsid)
  - [PUT /subjects/:id](#put-subjectsid)
  - [DELETE /subjects/:id](#delete-subjectsid)
- [Teachers](#teachers)
  - [GET /teachers](#get-teachers)
  - [POST /teachers](#post-teachers)
  - [GET /teachers/:id](#get-teachersid)
  - [PUT /teachers/:id](#put-teachersid)
- [Students](#students)
  - [GET /students](#get-students)
  - [POST /students](#post-students)
  - [GET /students/:id](#get-studentsid)
  - [PUT /students/:id](#put-studentsid)
- [Classes](#classes)
  - [GET /classes](#get-classes)
  - [POST /classes](#post-classes)
  - [GET /classes/:id](#get-classesid)
  - [PUT /classes/:id](#put-classesid)
  - [GET /classes/available-teachers](#get-classesavailable-teachers)
- [Class Students](#class-students)
  - [GET /classes/:id/students](#get-classesidstudents)
  - [GET /classes/:id/available-students](#get-classesidavailable-students)
  - [POST /classes/:id/students](#post-classesidstudents)
  - [DELETE /classes/:id/students/:student_id](#delete-classesidstudentsstudent_id)
- [Subject Assignments](#subject-assignments)
  - [GET /classes/:id/assignments](#get-classesidassignments)
  - [POST /classes/:id/assignments](#post-classesidassignments)
  - [PUT /classes/:id/assignments/:assignment_id](#put-classesidassignmentsassignment_id)
  - [DELETE /classes/:id/assignments/:assignment_id](#delete-classesidassignmentsassignment_id)
- [Teacher Dashboard](#teacher-dashboard)
  - [GET /teacher/dashboard](#get-teacherdashboard)
  - [GET /teacher/homeroom/:class_id/students](#get-teacherhomeroomclass_idstudents)
  - [GET /teacher/homeroom/:class_id/score-overview](#get-teacherhomeroomclass_idscore-overview)
  - [GET /teacher/homeroom/:class_id/students/:student_id/score-detail](#get-teacherhomeroomclass_idstudentsstudent_idscore-detail)
  - [GET /teacher/homeroom/:class_id/students/:student_id/comments](#get-teacherhomeroomclass_idstudentsstudent_idcomments)
  - [POST /teacher/homeroom/:class_id/students/:student_id/comments](#post-teacherhomeroomclass_idstudentsstudent_idcomments)
  - [PUT /teacher/homeroom/:class_id/comments/:comment_id](#put-teacherhomeroomclass_idcommentscomment_id)
  - [GET /teacher/assignments/:assignment_id/scores](#get-teacherassignmentsassignment_idscores)
  - [POST /teacher/assignments/:assignment_id/score-columns](#post-teacherassignmentsassignment_idscore-columns)
  - [PUT /teacher/assignments/:assignment_id/scores](#put-teacherassignmentsassignment_idscores)
- [Portal — Phụ huynh & Học sinh](#portal--phụ-huynh--học-sinh)
  - [GET /portal/parent](#get-portalparent)
  - [GET /portal/parent/students/:student_id](#get-portalparentstudentsstudent_id)
  - [GET /portal/student](#get-portalstudent)

---

## Quy ước chung

### Cấu trúc Response

Tất cả API đều trả về cùng một cấu trúc JSON.

**Success Response**
```json
{
  "success": true,
  "message": "Mô tả kết quả",
  "data": { }
}
```

**Error Response**
```json
{
  "success": false,
  "message": "Mô tả lỗi"
}
```

**Validation Error Response** (HTTP 422)
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "fieldName": ["Thông báo lỗi chi tiết"]
  }
}
```

### HTTP Status Codes

| Code | Ý nghĩa |
|------|---------|
| `200` | Thành công |
| `201` | Tạo mới thành công |
| `400` | Bad Request – dữ liệu không hợp lệ về mặt logic |
| `401` | Unauthorized – xác thực thất bại |
| `403` | Forbidden – không đủ quyền |
| `404` | Not Found – tài nguyên không tồn tại |
| `409` | Conflict – vi phạm business rule |
| `422` | Unprocessable Entity – lỗi validation (Zod) |
| `500` | Internal Server Error |

### Roles

| Giá trị | Mô tả |
|---------|-------|
| `admin_system` | Quản trị viên hệ thống (không thuộc trường cụ thể) |
| `admin_school` | Quản trị viên trường |
| `teacher` | Giáo viên |
| `parent` | Phụ huynh |
| `student` | Học sinh |

---

## Health Check

### GET /health

Kiểm tra trạng thái server.

**Request:** Không có body.

**Response 200**
```json
{
  "success": true,
  "message": "Server is running",
  "timestamp": "2026-02-25T00:00:00.000Z"
}
```

---

## Auth

### POST /auth/login

> **FR-001** – Đăng nhập hệ thống

Xác thực tài khoản bằng email và mật khẩu. Trả về JWT access token và thông tin người dùng.

**Business Rules:**
- `BR-001-01`: Phân quyền theo `role` của tài khoản.
- `BR-001-02`: Mỗi tài khoản có đúng 1 role.
- Thông báo lỗi dùng chung cho cả email không tồn tại và sai mật khẩu (tránh lộ thông tin).

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `email` | `string` | Có | Địa chỉ email hợp lệ |
| `password` | `string` | Có | Mật khẩu |

```json
{
  "email": "teacher@school.edu.vn",
  "password": "secret123"
}
```

**Response 200 – Đăng nhập thành công**
```json
{
  "success": true,
  "message": "Đăng nhập thành công",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "teacher@school.edu.vn",
      "role": "teacher",
      "school_id": "660e8400-e29b-41d4-a716-446655440001"
    }
  }
}
```

> `school_id` là `null` khi `role = admin_system`.

**Response 401 – Sai thông tin đăng nhập**
```json
{
  "success": false,
  "message": "Email hoặc mật khẩu không đúng"
}
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "email": ["Vui lòng nhập email"],
    "password": ["Vui lòng nhập mật khẩu"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `email` | Bắt buộc | "Vui lòng nhập email" |
| `email` | Đúng định dạng email | "Vui lòng nhập đúng định dạng email" |
| `password` | Bắt buộc | "Vui lòng nhập mật khẩu" |

---

### POST /auth/forgot-password

> **FR-018 (Phần 1)** – Yêu cầu đặt lại mật khẩu

Gửi email chứa link đặt lại mật khẩu đến địa chỉ email của người dùng.

**Business Rules:**
- `BR-018-01`: Token có hiệu lực trong **15 phút** kể từ lúc tạo.
- `BR-018-03`: Tiêu đề email: "Đặt lại mật khẩu".
- `BR-018-04`: Nội dung email chứa link reset theo format `{FRONTEND_URL}/reset-password?token={token}`.

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `email` | `string` | Có | Địa chỉ email đã đăng ký trong hệ thống |

```json
{
  "email": "teacher@school.edu.vn"
}
```

**Response 200 – Email đã được gửi**
```json
{
  "success": true,
  "message": "Vui lòng kiểm tra email để đặt lại mật khẩu",
  "data": null
}
```

**Response 404 – Email không tồn tại**
```json
{
  "success": false,
  "message": "Email không tồn tại trong hệ thống"
}
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "email": ["Vui lòng nhập email"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `email` | Bắt buộc | "Vui lòng nhập email" |
| `email` | Đúng định dạng email | "Email không đúng định dạng" |

---

### POST /auth/reset-password

> **FR-018 (Phần 2)** – Đặt lại mật khẩu bằng token

Xác nhận token từ email và cập nhật mật khẩu mới cho tài khoản. Token bị vô hiệu hóa ngay sau khi sử dụng thành công.

**Business Rules:**
- `BR-018-02`: Token bị vô hiệu hóa sau khi dùng (ghi nhận `used_at`).
- Token hết hạn nếu đã quá 15 phút hoặc đã được sử dụng trước đó.
- Cập nhật mật khẩu và đánh dấu token trong cùng một transaction (atomic).

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `token` | `string` | Có | Token nhận từ link email |
| `password` | `string` | Có | Mật khẩu mới |
| `confirmPassword` | `string` | Có | Xác nhận mật khẩu mới (phải khớp với `password`) |

```json
{
  "token": "a3f4b2c1d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2",
  "password": "newSecret456",
  "confirmPassword": "newSecret456"
}
```

**Response 200 – Đặt lại mật khẩu thành công**
```json
{
  "success": true,
  "message": "Đặt lại mật khẩu thành công",
  "data": null
}
```

**Response 400 – Token hết hạn hoặc đã sử dụng**
```json
{
  "success": false,
  "message": "Link đặt lại mật khẩu đã hết hạn"
}
```

> Lỗi 400 áp dụng cho cả 3 trường hợp: token không tồn tại, token đã dùng, và token quá 15 phút.

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "confirmPassword": ["Mật khẩu mới và xác nhận mật khẩu không trùng khớp"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `token` | Bắt buộc, không rỗng | "Token không hợp lệ" |
| `password` | Bắt buộc | "Vui lòng nhập mật khẩu mới" |
| `confirmPassword` | Bắt buộc | "Vui lòng xác nhận mật khẩu mới" |
| `confirmPassword` | Phải khớp với `password` | "Mật khẩu mới và xác nhận mật khẩu không trùng khớp" |

---

## Error toàn cục

| HTTP | Thông báo | Khi nào |
|------|-----------|---------|
| `422` | "Vui lòng kiểm tra lại thông tin" | Zod validation thất bại – kèm field `errors` |
| `500` | "Lỗi hệ thống, vui lòng thử lại sau" | Lỗi không mong đợi phía server |

---

## Schools

> **Actor:** Admin System
> **Auth:** Yêu cầu JWT Bearer token với role `admin_system`

### GET /schools

> **FR-002 – BR-002-06, BR-002-07** – Lấy danh sách trường học

**Business Rules:**
- `BR-002-06`: Tìm kiếm theo tên hoặc mã trường, không phân biệt hoa thường.
- `BR-002-07`: Ô tìm kiếm để trống → trả về toàn bộ danh sách, sắp xếp theo `created_at` mới nhất trước.

**Query Parameters**

| Parameter | Type | Bắt buộc | Mô tả |
|-----------|------|----------|-------|
| `search` | `string` | Không | Từ khóa tìm theo tên hoặc mã trường |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách trường học thành công",
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Trường THPT Lê Hồng Phong",
      "school_code": "THPTLHP",
      "created_at": "2026-01-15T08:00:00.000Z",
      "updated_at": "2026-01-15T08:00:00.000Z"
    }
  ]
}
```

---

### POST /schools

> **FR-002 – BR-002-01, BR-002-02** – Tạo trường học mới

**Business Rules:**
- `BR-002-01`: Tự động tạo tài khoản `admin_school` khi tạo trường. Mật khẩu tạm được log ra console (dev) hoặc gửi email (production).
- `BR-002-02`: `school_code` phải unique trong hệ thống.
- Tạo trường và tài khoản Admin School trong cùng một transaction (atomic).

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `name` | `string` | Có | Tên trường |
| `school_code` | `string` | Có | Mã trường (tối đa 8 ký tự, chỉ chữ và số) |
| `admin_email` | `string` | Có | Email tài khoản Admin School sẽ được tạo |

```json
{
  "name": "Trường THPT Lê Hồng Phong",
  "school_code": "THPTLHP",
  "admin_email": "admin@thptlhp.edu.vn"
}
```

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo trường học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Trường THPT Lê Hồng Phong",
    "school_code": "THPTLHP",
    "created_at": "2026-01-15T08:00:00.000Z",
    "updated_at": "2026-01-15T08:00:00.000Z"
  }
}
```

**Response 409 – Mã trường đã tồn tại**
```json
{
  "success": false,
  "message": "Mã trường đã tồn tại trong hệ thống"
}
```

**Response 409 – Email Admin School đã tồn tại**
```json
{
  "success": false,
  "message": "Email Admin School đã tồn tại trong hệ thống"
}
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "name": ["Vui lòng nhập tên trường"],
    "school_code": ["Vui lòng nhập mã trường"],
    "admin_email": ["Vui lòng nhập email Admin School"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `name` | Bắt buộc | "Vui lòng nhập tên trường" |
| `school_code` | Bắt buộc | "Vui lòng nhập mã trường" |
| `school_code` | Tối đa 8 ký tự | "Mã trường không được vượt quá 8 ký tự" |
| `school_code` | Chỉ chữ cái và chữ số (`^[a-zA-Z0-9]+$`) | "Mã trường chỉ được chứa chữ cái và chữ số" |
| `admin_email` | Bắt buộc | "Vui lòng nhập email Admin School" |
| `admin_email` | Đúng định dạng email | "Email không đúng định dạng" |

---

### GET /schools/:id

> **FR-002** – Lấy chi tiết một trường học

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của trường học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin trường học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Trường THPT Lê Hồng Phong",
    "school_code": "THPTLHP",
    "created_at": "2026-01-15T08:00:00.000Z",
    "updated_at": "2026-01-15T08:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy trường**
```json
{
  "success": false,
  "message": "Không tìm thấy trường học phù hợp"
}
```

---

### PUT /schools/:id

> **FR-002 – BR-002-02** – Cập nhật thông tin trường học

**Business Rules:**
- `BR-002-02`: Nếu thay đổi `school_code`, kiểm tra không trùng với trường khác.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của trường học |

**Request Body** (ít nhất một field)

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `name` | `string` | Không | Tên trường mới |
| `school_code` | `string` | Không | Mã trường mới (tối đa 8 ký tự, chỉ chữ và số) |

```json
{
  "name": "Trường THPT Lê Hồng Phong (cập nhật)",
  "school_code": "LHP01"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật trường học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Trường THPT Lê Hồng Phong (cập nhật)",
    "school_code": "LHP01",
    "created_at": "2026-01-15T08:00:00.000Z",
    "updated_at": "2026-02-25T10:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy trường**
```json
{
  "success": false,
  "message": "Không tìm thấy trường học phù hợp"
}
```

**Response 409 – Mã trường đã tồn tại**
```json
{
  "success": false,
  "message": "Mã trường đã tồn tại trong hệ thống"
}
```

**Validation Rules** (chỉ áp dụng nếu field được gửi lên)

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `school_code` | Tối đa 8 ký tự | "Mã trường không được vượt quá 8 ký tự" |
| `school_code` | Chỉ chữ cái và chữ số | "Mã trường chỉ được chứa chữ cái và chữ số" |

---

### DELETE /schools/:id

> **FR-002 – BR-002-05** – Xóa trường học

**Business Rules:**
- `BR-002-05`: Không cho xóa nếu trường đã có **master data** (giáo viên, học sinh, hoặc năm học). Nếu chưa có → xóa cứng cả tài khoản Admin School và trường.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của trường học |

**Response 200 – Xóa thành công**
```json
{
  "success": true,
  "message": "Xóa trường học thành công",
  "data": null
}
```

**Response 404 – Không tìm thấy trường**
```json
{
  "success": false,
  "message": "Không tìm thấy trường học phù hợp"
}
```

**Response 409 – Còn master data**
```json
{
  "success": false,
  "message": "Không thể xóa trường học này vì đã có dữ liệu (giáo viên, học sinh hoặc năm học)"
}
```

---

## Academic Years

> **Actor:** Admin School
> **Auth:** Yêu cầu JWT Bearer token với role `admin_school`
> `school_id` tự động lấy từ token — Admin School chỉ thao tác được trên trường của mình.

### Trạng thái năm học

| Giá trị | Hiển thị | Chuyển sang trạng thái kế tiếp |
|---------|----------|-------------------------------|
| `preparing` | Chuẩn bị diễn ra | Tự động → `in_progress` khi đến `start_date` |
| `in_progress` | Đang diễn ra | Tự động → `completed` khi qua `end_date` |
| `completed` | Đã kết thúc | Trạng thái cuối — không chuyển tiếp |

**Cơ chế tự động (Scheduler):**
- Server chạy scheduler mỗi **1 giờ**, cũng chạy ngay khi khởi động.
- `preparing` → `in_progress`: khi `start_date ≤ hôm nay` **và** trường chưa có năm học `in_progress` nào (BR-003-01).
- `in_progress` → `completed`: khi `end_date < hôm nay`.
- Trạng thái **chỉ được thay đổi bởi scheduler** — Admin School không được đặt `status` thủ công.

---

### GET /academic-years

> **FR-003 – BR-003-02** – Lấy danh sách năm học của trường

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách năm học thành công",
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "2025-2026",
      "start_date": "2025-09-01T00:00:00.000Z",
      "end_date": "2026-05-31T00:00:00.000Z",
      "status": "in_progress",
      "created_at": "2025-08-01T08:00:00.000Z",
      "updated_at": "2025-09-01T08:00:00.000Z"
    }
  ]
}
```

**Response 401 – Chưa đăng nhập**
```json
{ "success": false, "message": "Bạn chưa đăng nhập" }
```

**Response 403 – Sai role**
```json
{ "success": false, "message": "Bạn không có quyền thực hiện hành động này" }
```

---

### POST /academic-years

> **FR-003** – Tạo năm học mới

**Business Rules:**
- Trạng thái được tính tự động từ ngày hiện tại tại thời điểm tạo:
  - `end_date ≤ hôm nay` → `completed`
  - `start_date ≤ hôm nay < end_date` → `in_progress`
  - `hôm nay < start_date` → `preparing`
- Không cần truyền `status` — hệ thống tự gán.
- `BR-003-name`: Tên năm học phải unique trong trường (case-insensitive).
- `BR-003-overlap`: Khoảng thời gian không được trùng với bất kỳ năm học nào khác trong trường (kiểm tra cả boundary: `start_date` và `end_date` chạm nhau cũng là trùng).

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `name` | `string` | Có | Tên năm học (VD: "2025-2026") |
| `start_date` | `string` (ISO date) | Có | Ngày bắt đầu |
| `end_date` | `string` (ISO date) | Có | Ngày kết thúc (phải sau `start_date`) |

```json
{
  "name": "2025-2026",
  "start_date": "2025-09-01",
  "end_date": "2026-05-31"
}
```

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo năm học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "2025-2026",
    "start_date": "2025-09-01T00:00:00.000Z",
    "end_date": "2026-05-31T00:00:00.000Z",
    "status": "in_progress",
    "created_at": "2025-08-01T08:00:00.000Z",
    "updated_at": "2025-08-01T08:00:00.000Z"
  }
}
```

**Response 409 – Tên năm học đã tồn tại**
```json
{
  "success": false,
  "message": "Tên năm học đã tồn tại trong trường này"
}
```

**Response 409 – Thời gian trùng với năm học khác**
```json
{
  "success": false,
  "message": "Thời gian năm học bị trùng với một năm học khác"
}
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "name": ["Vui lòng nhập tên năm học"],
    "start_date": ["Vui lòng chọn thời gian bắt đầu"],
    "end_date": ["Thời gian kết thúc phải sau thời gian bắt đầu"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `name` | Bắt buộc | "Vui lòng nhập tên năm học" |
| `start_date` | Bắt buộc, định dạng date | "Vui lòng chọn thời gian bắt đầu" |
| `end_date` | Bắt buộc, định dạng date | "Vui lòng chọn thời gian kết thúc" |
| `end_date` | Phải sau `start_date` | "Thời gian kết thúc phải sau thời gian bắt đầu" |

---

### GET /academic-years/:id

> **FR-003** – Lấy chi tiết một năm học

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của năm học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin năm học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "2025-2026",
    "start_date": "2025-09-01T00:00:00.000Z",
    "end_date": "2026-05-31T00:00:00.000Z",
    "status": "in_progress",
    "created_at": "2025-08-01T08:00:00.000Z",
    "updated_at": "2025-09-01T08:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy năm học phù hợp" }
```

---

### PUT /academic-years/:id

> **FR-003** – Cập nhật thông tin năm học

**Business Rules:**
- Không thể cập nhật năm học có trạng thái `completed`.
- Tên phải unique trong trường (case-insensitive), trừ chính nó.
- Khi chỉ cập nhật một trong `start_date`/`end_date`, hệ thống tự so sánh với giá trị còn lại trong DB để đảm bảo `end_date > start_date`.
- Khoảng thời gian mới không được trùng với năm học khác (kể cả boundary).
- Không được truyền `status` — trạng thái chỉ do scheduler tự động quản lý.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của năm học |

**Request Body** (ít nhất một field)

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `name` | `string` | Không | Tên năm học mới |
| `start_date` | `string` (ISO date) | Không | Ngày bắt đầu mới |
| `end_date` | `string` (ISO date) | Không | Ngày kết thúc mới |

```json
{
  "name": "2025-2026 (cập nhật)",
  "end_date": "2026-06-15"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật năm học thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "2025-2026 (cập nhật)",
    "start_date": "2025-09-01T00:00:00.000Z",
    "end_date": "2026-06-15T00:00:00.000Z",
    "status": "in_progress",
    "created_at": "2025-08-01T08:00:00.000Z",
    "updated_at": "2025-09-02T08:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy năm học phù hợp" }
```

**Response 409 – Năm học đã kết thúc**
```json
{ "success": false, "message": "Không thể chỉnh sửa năm học đã kết thúc" }
```

**Response 409 – Tên năm học đã tồn tại**
```json
{ "success": false, "message": "Tên năm học đã tồn tại trong trường này" }
```

**Response 409 – Thời gian trùng với năm học khác**
```json
{ "success": false, "message": "Thời gian năm học bị trùng với một năm học khác" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "end_date": ["Thời gian kết thúc phải sau thời gian bắt đầu"]
  }
}
```

---

### DELETE /academic-years/:id

> **FR-003** – Xóa năm học

**Business Rules:**
- Chỉ được xóa khi năm học ở trạng thái `preparing`. Năm học `in_progress` hoặc `completed` không thể xóa.
- Xóa cứng khỏi DB.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của năm học |

**Response 200 – Xóa thành công**
```json
{
  "success": true,
  "message": "Xóa năm học thành công",
  "data": null
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy năm học phù hợp" }
```

**Response 409 – Không được phép xóa**
```json
{ "success": false, "message": "Chỉ có thể xóa năm học ở trạng thái Chuẩn bị diễn ra" }
```

---

## Subjects

> **Actor:** Admin School
> **Auth:** Yêu cầu JWT Bearer token với role `admin_school`
> `school_id` tự động lấy từ token — Admin School chỉ thao tác được trên môn học thuộc trường của mình.

### GET /subjects

> **FR-004** – Lấy danh sách môn học theo năm học

**Business Rules:**
- `BR-004-01`: `academic_year_id` phải thuộc trường của admin_school đang đăng nhập.
- Kết quả sắp xếp theo tên môn học (A → Z).

**Query Parameters**

| Parameter | Type | Bắt buộc | Mô tả |
|-----------|------|----------|-------|
| `academic_year_id` | `string` (UUID) | Có | ID của năm học cần lấy danh sách môn |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách môn học thành công",
  "data": [
    {
      "id": "770e8400-e29b-41d4-a716-446655440000",
      "name": "Toán",
      "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
      "created_at": "2026-01-10T08:00:00.000Z",
      "updated_at": "2026-01-10T08:00:00.000Z"
    },
    {
      "id": "880e8400-e29b-41d4-a716-446655440000",
      "name": "Văn",
      "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
      "created_at": "2026-01-10T08:00:00.000Z",
      "updated_at": "2026-01-10T08:00:00.000Z"
    }
  ]
}
```

**Response 400 – Thiếu academic_year_id**
```json
{ "success": false, "message": "Vui lòng cung cấp academic_year_id" }
```

**Response 404 – Năm học không tồn tại hoặc không thuộc trường**
```json
{ "success": false, "message": "Không tìm thấy năm học phù hợp" }
```

---

### POST /subjects

> **FR-004** – Tạo môn học mới

**Business Rules:**
- `BR-004-01`: `academic_year_id` phải thuộc trường của admin_school đang đăng nhập.
- `BR-004-01b`: Không thể tạo môn học trong năm học có trạng thái `completed`.
- `BR-004-02`: Tên môn học phải unique trong cùng năm học (case-insensitive).

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `academic_year_id` | `string` (UUID) | Có | ID của năm học |
| `name` | `string` | Có | Tên môn học |

```json
{
  "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Toán"
}
```

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo môn học thành công",
  "data": {
    "id": "770e8400-e29b-41d4-a716-446655440000",
    "name": "Toán",
    "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-01-10T08:00:00.000Z"
  }
}
```

**Response 404 – Năm học không tồn tại hoặc không thuộc trường**
```json
{ "success": false, "message": "Không tìm thấy năm học phù hợp" }
```

**Response 409 – Năm học đã kết thúc**
```json
{ "success": false, "message": "Không thể thêm môn học vào năm học đã kết thúc" }
```

**Response 409 – Tên môn học đã tồn tại trong năm học**
```json
{ "success": false, "message": "Tên môn học đã tồn tại trong năm học này" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "name": ["Vui lòng nhập tên môn học"],
    "academic_year_id": ["Năm học không hợp lệ"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `academic_year_id` | Bắt buộc, UUID hợp lệ | "Năm học không hợp lệ" |
| `name` | Bắt buộc | "Vui lòng nhập tên môn học" |

---

### GET /subjects/:id

> **FR-004** – Lấy chi tiết một môn học

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của môn học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin môn học thành công",
  "data": {
    "id": "770e8400-e29b-41d4-a716-446655440000",
    "name": "Toán",
    "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-01-10T08:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy môn học phù hợp" }
```

---

### PUT /subjects/:id

> **FR-004** – Cập nhật tên môn học

**Business Rules:**
- `BR-004-02`: Tên mới phải unique trong cùng năm học (case-insensitive), trừ chính môn học đó.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của môn học |

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `name` | `string` | Có | Tên môn học mới |

```json
{
  "name": "Toán học"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật môn học thành công",
  "data": {
    "id": "770e8400-e29b-41d4-a716-446655440000",
    "name": "Toán học",
    "academic_year_id": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-02-10T08:00:00.000Z"
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy môn học phù hợp" }
```

**Response 409 – Tên môn học đã tồn tại trong năm học**
```json
{ "success": false, "message": "Tên môn học đã tồn tại trong năm học này" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "name": ["Vui lòng nhập tên môn học"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `name` | Bắt buộc | "Vui lòng nhập tên môn học" |

---

### DELETE /subjects/:id

> **FR-004** – Xóa môn học

**Business Rules:**
- `BR-004-03`: Không được xóa nếu môn học đã có phân công giảng dạy (`SubjectAssignment`).
- Xóa cứng khỏi DB.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của môn học |

**Response 200 – Xóa thành công**
```json
{
  "success": true,
  "message": "Xóa môn học thành công",
  "data": null
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy môn học phù hợp" }
```

**Response 409 – Đã có phân công giảng dạy**
```json
{ "success": false, "message": "Không thể xóa môn học này vì đã có phân công giảng dạy" }
```

---

## Teachers

> **Actor:** Admin School
> **Auth:** Yêu cầu JWT Bearer token với role `admin_school`
> `school_id` tự động lấy từ token — Admin School chỉ thao tác được trên giáo viên thuộc trường của mình.

### Cấu trúc đối tượng Teacher

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "full_name": "Nguyễn Văn A",
  "date_of_birth": "1985-03-15T00:00:00.000Z",
  "address": "123 Lê Lợi, Quận 1, TP. HCM",
  "citizen_id": "079123456789",
  "teacher_code": "GV001",
  "gender": "male",
  "work_status": "active",
  "created_at": "2026-01-10T08:00:00.000Z",
  "updated_at": "2026-01-10T08:00:00.000Z",
  "user": {
    "id": "660e8400-e29b-41d4-a716-446655440001",
    "email": "giaovien@truong.edu.vn"
  }
}
```

| Field | Type | Mô tả |
|-------|------|-------|
| `id` | `string` (UUID) | ID giáo viên |
| `full_name` | `string` | Họ và tên |
| `date_of_birth` | `string` (ISO datetime) | Ngày sinh |
| `address` | `string` | Địa chỉ |
| `citizen_id` | `string` | Số CCCD (unique toàn hệ thống) |
| `teacher_code` | `string` | Mã số giáo viên (unique toàn hệ thống) |
| `gender` | `"male" \| "female" \| "other"` | Giới tính |
| `work_status` | `"active" \| "resigned"` | Trạng thái làm việc |
| `user.email` | `string` | Email đăng nhập của giáo viên |

---

### GET /teachers

> **FR-005** – Lấy danh sách giáo viên của trường

**Business Rules:**
- Trả về danh sách giáo viên thuộc trường của admin_school đang đăng nhập.
- Hỗ trợ tìm kiếm theo họ tên hoặc mã giáo viên, không phân biệt hoa thường.
- Ô tìm kiếm để trống → trả về toàn bộ, sắp xếp theo `full_name` A → Z.

**Query Parameters**

| Parameter | Type | Bắt buộc | Mô tả |
|-----------|------|----------|-------|
| `search` | `string` | Không | Từ khóa tìm theo họ tên hoặc mã GV |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách giáo viên thành công",
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "full_name": "Nguyễn Văn A",
      "date_of_birth": "1985-03-15T00:00:00.000Z",
      "address": "123 Lê Lợi, Quận 1, TP. HCM",
      "citizen_id": "079123456789",
      "teacher_code": "GV001",
      "gender": "male",
      "work_status": "active",
      "created_at": "2026-01-10T08:00:00.000Z",
      "updated_at": "2026-01-10T08:00:00.000Z",
      "user": { "id": "660e8400-...", "email": "giaovien@truong.edu.vn" }
    }
  ]
}
```

**Response 401 – Chưa đăng nhập**
```json
{ "success": false, "message": "Bạn chưa đăng nhập" }
```

**Response 403 – Sai role**
```json
{ "success": false, "message": "Bạn không có quyền thực hiện hành động này" }
```

---

### POST /teachers

> **FR-005** – Tạo giáo viên mới

**Business Rules:**
- `BR-005-01`: `citizen_id` phải unique **toàn hệ thống** (không chỉ trong trường).
- `BR-005-02`: `teacher_code` phải unique **toàn hệ thống**.
- `BR-005-03`: Tạo tài khoản `User` (role=`teacher`) và hồ sơ `Teacher` trong cùng một transaction (atomic).
- `work_status` mặc định là `active` nếu không truyền.

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `full_name` | `string` | Có | Họ và tên giáo viên |
| `date_of_birth` | `string` (ISO date) | Có | Ngày sinh (`YYYY-MM-DD`) |
| `address` | `string` | Có | Địa chỉ |
| `citizen_id` | `string` | Có | Số CCCD |
| `teacher_code` | `string` | Có | Mã số giáo viên (VD: GV001) |
| `gender` | `"male" \| "female" \| "other"` | Có | Giới tính |
| `work_status` | `"active" \| "resigned"` | Không | Trạng thái làm việc (mặc định: `active`) |
| `email` | `string` | Có | Email đăng nhập của giáo viên |
| `password` | `string` | Có | Mật khẩu đăng nhập |

```json
{
  "full_name": "Nguyễn Văn A",
  "date_of_birth": "1985-03-15",
  "address": "123 Lê Lợi, Quận 1, TP. HCM",
  "citizen_id": "079123456789",
  "teacher_code": "GV001",
  "gender": "male",
  "work_status": "active",
  "email": "giaovien@truong.edu.vn",
  "password": "matkhau123"
}
```

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo giáo viên thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "full_name": "Nguyễn Văn A",
    "date_of_birth": "1985-03-15T00:00:00.000Z",
    "address": "123 Lê Lợi, Quận 1, TP. HCM",
    "citizen_id": "079123456789",
    "teacher_code": "GV001",
    "gender": "male",
    "work_status": "active",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-01-10T08:00:00.000Z",
    "user": { "id": "660e8400-...", "email": "giaovien@truong.edu.vn" }
  }
}
```

**Response 409 – Số CCCD đã tồn tại**
```json
{ "success": false, "message": "Số CCCD đã tồn tại trong hệ thống" }
```

**Response 409 – Mã số giáo viên đã tồn tại**
```json
{ "success": false, "message": "Mã số giáo viên đã tồn tại trong hệ thống" }
```

**Response 409 – Email đã tồn tại**
```json
{ "success": false, "message": "Email đã tồn tại trong hệ thống" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "full_name": ["Vui lòng nhập họ tên"],
    "date_of_birth": ["Vui lòng nhập ngày sinh"],
    "gender": ["Vui lòng chọn giới tính"],
    "email": ["Vui lòng nhập email"],
    "password": ["Vui lòng nhập mật khẩu"]
  }
}
```

**Validation Rules**

| Field | Rule | Thông báo lỗi |
|-------|------|---------------|
| `full_name` | Bắt buộc | "Vui lòng nhập họ tên" |
| `date_of_birth` | Bắt buộc, định dạng date | "Vui lòng nhập ngày sinh" |
| `date_of_birth` | Không được là ngày trong tương lai | "Ngày sinh không được là ngày trong tương lai" |
| `address` | Bắt buộc | "Vui lòng nhập địa chỉ" |
| `citizen_id` | Bắt buộc | "Vui lòng nhập số CCCD" |
| `citizen_id` | Tối đa 12 ký tự | "Số CCCD không được vượt quá 12 ký tự" |
| `citizen_id` | Chỉ chứa chữ số (`^\d+$`) | "Số CCCD chỉ được chứa chữ số" |
| `teacher_code` | Bắt buộc | "Vui lòng nhập mã số giáo viên" |
| `gender` | Bắt buộc, một trong `male/female/other` | "Vui lòng chọn giới tính" |
| `email` | Bắt buộc | "Vui lòng nhập email" |
| `email` | Đúng định dạng email | "Email không đúng định dạng" |
| `password` | Bắt buộc | "Vui lòng nhập mật khẩu" |

---

### GET /teachers/:id

> **FR-005** – Lấy chi tiết một giáo viên

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của giáo viên |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin giáo viên thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "full_name": "Nguyễn Văn A",
    "date_of_birth": "1985-03-15T00:00:00.000Z",
    "address": "123 Lê Lợi, Quận 1, TP. HCM",
    "citizen_id": "079123456789",
    "teacher_code": "GV001",
    "gender": "male",
    "work_status": "active",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-01-10T08:00:00.000Z",
    "user": { "id": "660e8400-...", "email": "giaovien@truong.edu.vn" }
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy giáo viên phù hợp" }
```

---

### PUT /teachers/:id

> **FR-005** – Cập nhật thông tin giáo viên

**Business Rules:**
- `BR-005-01`: Nếu thay đổi `citizen_id`, kiểm tra unique toàn hệ thống (trừ chính giáo viên đó).
- `BR-005-02`: Nếu thay đổi `teacher_code`, kiểm tra unique toàn hệ thống (trừ chính giáo viên đó).
- Không thể thay đổi `email` hoặc `password` qua endpoint này (dùng chức năng quên mật khẩu).
- Admin School chỉ cập nhật được giáo viên thuộc trường của mình.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID của giáo viên |

**Request Body** (ít nhất một field)

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `full_name` | `string` | Không | Họ và tên mới |
| `date_of_birth` | `string` (ISO date) | Không | Ngày sinh mới (`YYYY-MM-DD`) |
| `address` | `string` | Không | Địa chỉ mới |
| `citizen_id` | `string` | Không | Số CCCD mới |
| `teacher_code` | `string` | Không | Mã số giáo viên mới |
| `gender` | `"male" \| "female" \| "other"` | Không | Giới tính mới |
| `work_status` | `"active" \| "resigned"` | Không | Trạng thái làm việc mới |

```json
{
  "full_name": "Nguyễn Văn B",
  "work_status": "resigned"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật giáo viên thành công",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "full_name": "Nguyễn Văn B",
    "date_of_birth": "1985-03-15T00:00:00.000Z",
    "address": "123 Lê Lợi, Quận 1, TP. HCM",
    "citizen_id": "079123456789",
    "teacher_code": "GV001",
    "gender": "male",
    "work_status": "resigned",
    "created_at": "2026-01-10T08:00:00.000Z",
    "updated_at": "2026-02-28T10:00:00.000Z",
    "user": { "id": "660e8400-...", "email": "giaovien@truong.edu.vn" }
  }
}
```

**Response 404 – Không tìm thấy**
```json
{ "success": false, "message": "Không tìm thấy giáo viên phù hợp" }
```

**Response 409 – Số CCCD đã tồn tại**
```json
{ "success": false, "message": "Số CCCD đã tồn tại trong hệ thống" }
```

**Response 409 – Mã số giáo viên đã tồn tại**
```json
{ "success": false, "message": "Mã số giáo viên đã tồn tại trong hệ thống" }
```

---

## Students

> **Yêu cầu xác thực:** `Authorization: Bearer <token>`
> **Quyền:** `admin_school` (school_id lấy từ JWT)

### GET /students

Lấy danh sách học sinh của trường. Hỗ trợ tìm kiếm và lọc.

**Query Params**

| Tham số | Kiểu | Bắt buộc | Mô tả |
|---------|------|----------|-------|
| `search` | string | Không | Tìm theo họ tên hoặc mã học sinh (không phân biệt hoa thường) |
| `study_status` | `studying` \| `dropped_out` | Không | Lọc theo trạng thái học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách học sinh thành công",
  "data": [
    {
      "id": "7f22c1af-...",
      "student_code": "TSTEST2026001",
      "full_name": "Nguyễn Văn An",
      "date_of_birth": "2010-05-15T00:00:00.000Z",
      "address": "123 Lê Lợi, Quận 1, TP.HCM",
      "gender": "male",
      "study_status": "studying",
      "created_at": "2026-02-28T06:55:30.278Z",
      "updated_at": "2026-02-28T06:55:30.278Z",
      "user": { "id": "...", "email": "an.nguyen@student.edu.vn" },
      "parent": {
        "id": "...",
        "father_name": "Nguyễn Văn Bình",
        "mother_name": "Trần Thị Cúc",
        "phone": "0987654321",
        "user": { "id": "...", "email": "phuyhuynh@parent.edu.vn" }
      }
    }
  ]
}
```

---

### POST /students

Tạo học sinh mới. Tự động sinh `student_code`, tạo tài khoản học sinh và phụ huynh, gửi email thiết lập mật khẩu.

**Business Rules**
- `BR-006-01`: `student_code` tự động theo format `[school_code][year][seq]` (ví dụ: `TSTEST2026001`)
- `BR-006-05`: Email phụ huynh — nếu đã tồn tại và có bản ghi parent → dùng lại; nếu email thuộc tài khoản khác → lỗi 409; nếu chưa tồn tại → tạo mới
- `BR-006-06`: Gửi email thiết lập mật khẩu cho học sinh (luôn gửi) và phụ huynh (chỉ khi tạo mới), token có hiệu lực 7 ngày

**Request Body**

```json
{
  "full_name": "Nguyễn Văn An",
  "date_of_birth": "2010-05-15",
  "address": "123 Lê Lợi, Quận 1, TP.HCM",
  "gender": "male",
  "email": "an.nguyen@student.edu.vn",
  "study_status": "studying",
  "parent": {
    "email": "phuyhuynh@parent.edu.vn",
    "phone": "0987654321",
    "father_name": "Nguyễn Văn Bình",
    "father_occupation": "Kỹ sư",
    "father_date_of_birth": "1975-03-10",
    "mother_name": "Trần Thị Cúc",
    "mother_occupation": "Kế toán",
    "mother_date_of_birth": "1978-07-22"
  }
}
```

**Validation Rules**

| Trường | Quy tắc |
|--------|---------|
| `full_name` | Bắt buộc |
| `date_of_birth` | Bắt buộc, định dạng `YYYY-MM-DD`, **không được là ngày trong tương lai** |
| `address` | Bắt buộc |
| `gender` | Bắt buộc, một trong `male` \| `female` \| `other` |
| `email` | Bắt buộc, đúng định dạng email, duy nhất |
| `study_status` | Không bắt buộc, mặc định `studying` |
| `parent.email` | Bắt buộc, đúng định dạng email |
| `parent.phone` | Bắt buộc, **chỉ được chứa chữ số** |
| `parent.father_name` | Bắt buộc |
| `parent.father_occupation` | Bắt buộc |
| `parent.father_date_of_birth` | Bắt buộc, định dạng `YYYY-MM-DD`, **không được là ngày trong tương lai** |
| `parent.mother_name` | Bắt buộc |
| `parent.mother_occupation` | Bắt buộc |
| `parent.mother_date_of_birth` | Bắt buộc, định dạng `YYYY-MM-DD`, **không được là ngày trong tương lai** |

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo học sinh thành công",
  "data": {
    "id": "7f22c1af-...",
    "student_code": "TSTEST2026001",
    "full_name": "Nguyễn Văn An",
    "date_of_birth": "2010-05-15T00:00:00.000Z",
    "address": "123 Lê Lợi, Quận 1, TP.HCM",
    "gender": "male",
    "study_status": "studying",
    "user": { "id": "...", "email": "an.nguyen@student.edu.vn" },
    "parent": {
      "id": "...",
      "father_name": "Nguyễn Văn Bình",
      "mother_name": "Trần Thị Cúc",
      "phone": "0987654321",
      "user": { "id": "...", "email": "phuyhuynh@parent.edu.vn" }
    }
  }
}
```

**Response 409 – Email học sinh đã tồn tại**
```json
{ "success": false, "message": "Email học sinh đã tồn tại trong hệ thống" }
```

**Response 409 – Email phụ huynh đã được dùng bởi tài khoản khác**
```json
{ "success": false, "message": "Email phụ huynh đã được sử dụng bởi tài khoản khác" }
```

---

### GET /students/:id

Lấy chi tiết một học sinh theo ID.

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin học sinh thành công",
  "data": { "id": "7f22c1af-...", "student_code": "TSTEST2026001", "..." }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy học sinh phù hợp" }
```

---

### PUT /students/:id

Cập nhật thông tin cơ bản của học sinh. Không thể thay đổi email.

**Request Body** (tất cả các trường đều không bắt buộc)

```json
{
  "full_name": "Nguyễn Văn An (Updated)",
  "date_of_birth": "2010-05-15",
  "address": "456 Nguyễn Huệ, Quận 1, TP.HCM",
  "gender": "male",
  "study_status": "dropped_out"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật học sinh thành công",
  "data": { "id": "7f22c1af-...", "..." }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy học sinh phù hợp" }
```

---

## Classes

> **FR-007: Quản lý lớp học**
> Actor: `admin_school` — `school_id` lấy từ JWT.

---

### GET /classes

Lấy danh sách lớp học của trường. Có thể lọc theo năm học và tìm kiếm theo tên lớp.

**Query Parameters**

| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| `academic_year_id` | string (uuid) | Lọc theo năm học |
| `search` | string | Tìm theo tên lớp (case-insensitive) |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách lớp học thành công",
  "data": [
    {
      "id": "uuid",
      "name": "10A1",
      "academic_year_id": "uuid",
      "created_at": "2026-01-01T00:00:00.000Z",
      "updated_at": "2026-01-01T00:00:00.000Z",
      "academic_year": { "id": "uuid", "name": "2025-2026" },
      "homeroom_teacher": { "id": "uuid", "full_name": "Nguyễn Văn A", "teacher_code": "GV001" }
    }
  ]
}
```

---

### POST /classes

Tạo lớp học mới.

**Request Body**

```json
{
  "academic_year_id": "uuid",
  "name": "10A1",
  "homeroom_teacher_id": "uuid"
}
```

**Validation Rules**

| Trường | Rule |
|--------|------|
| `academic_year_id` | Bắt buộc; phải thuộc trường của admin |
| `name` | Bắt buộc; không được trùng trong cùng năm học (BR-007-01) |
| `homeroom_teacher_id` | Bắt buộc; giáo viên chưa là GVCN lớp khác trong năm học (BR-007-02) |

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo lớp học thành công",
  "data": { "id": "uuid", "name": "10A1", "..." }
}
```

**Response 409 – Trùng tên lớp**
```json
{ "success": false, "message": "Tên lớp đã tồn tại trong năm học này" }
```

**Response 409 – Giáo viên đã là GVCN lớp khác**
```json
{ "success": false, "message": "Giáo viên này đã là chủ nhiệm lớp khác" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "name": ["Vui lòng nhập tên lớp"],
    "homeroom_teacher_id": ["Vui lòng chọn giáo viên chủ nhiệm"]
  }
}
```

---

### GET /classes/:id

Lấy chi tiết một lớp học.

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin lớp học thành công",
  "data": {
    "id": "uuid",
    "name": "10A1",
    "academic_year_id": "uuid",
    "academic_year": { "id": "uuid", "name": "2025-2026" },
    "homeroom_teacher": { "id": "uuid", "full_name": "Nguyễn Văn A", "teacher_code": "GV001" }
  }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

---

### PUT /classes/:id

Cập nhật tên lớp và/hoặc giáo viên chủ nhiệm.

**Request Body** (tất cả các trường đều không bắt buộc)

```json
{
  "name": "10A2",
  "homeroom_teacher_id": "uuid"
}
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật lớp học thành công",
  "data": { "id": "uuid", "name": "10A2", "..." }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

**Response 409**
```json
{ "success": false, "message": "Tên lớp đã tồn tại trong năm học này" }
```

---

### GET /classes/available-teachers

Lấy danh sách giáo viên chưa được gán làm GVCN cho bất kỳ lớp nào trong năm học (BR-007-03). Dùng khi tạo hoặc chỉnh sửa lớp để populate dropdown chọn GVCN.

**Query Parameters**

| Tham số | Kiểu | Bắt buộc | Mô tả |
|---------|------|----------|-------|
| `academic_year_id` | string (uuid) | Có | Năm học cần kiểm tra |
| `exclude_class_id` | string (uuid) | Không | Bỏ qua lớp này khi kiểm tra (dùng khi edit) |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách giáo viên khả dụng thành công",
  "data": [
    { "id": "uuid", "full_name": "Trần Thị B", "teacher_code": "GV002" }
  ]
}
```

**Response 400 – Thiếu academic_year_id**
```json
{ "success": false, "message": "Vui lòng chọn năm học" }
```

---

## Class Students

> **FR-008** – Quản lý danh sách học sinh trong lớp
> **Role yêu cầu:** `admin_school`
> **Business Rules:**
> - **BR-008-01:** 1 học sinh chỉ thuộc 1 lớp / năm học (enforce ở DB: `UNIQUE(academic_year_id, student_id)`)
> - **BR-008-02:** Chỉ thêm được học sinh có `study_status = 'studying'`
> - **BR-008-03:** Popup chỉ hiện học sinh studying và chưa có lớp trong năm học (`/available-students`)

---

### GET /classes/:id/students

Lấy danh sách học sinh đang thuộc lớp.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách học sinh trong lớp thành công",
  "data": [
    {
      "id": "uuid",
      "created_at": "2025-09-01T00:00:00.000Z",
      "student": {
        "id": "uuid",
        "student_code": "NXO20250001",
        "full_name": "Nguyễn Văn An",
        "date_of_birth": "2010-05-15T00:00:00.000Z",
        "gender": "male",
        "study_status": "studying"
      }
    }
  ]
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

---

### GET /classes/:id/available-students

Lấy danh sách học sinh khả dụng để thêm vào lớp (BR-008-02, BR-008-03).

Chỉ trả về học sinh:
- Thuộc trường (từ JWT)
- Có `study_status = 'studying'`
- Chưa thuộc lớp nào trong cùng năm học

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách học sinh khả dụng thành công",
  "data": [
    {
      "id": "uuid",
      "student_code": "NXO20250002",
      "full_name": "Trần Thị Bình",
      "date_of_birth": "2010-08-20T00:00:00.000Z",
      "gender": "female",
      "study_status": "studying"
    }
  ]
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

---

### POST /classes/:id/students

Thêm một hoặc nhiều học sinh vào lớp (batch).

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `student_ids` | `string[]` | Có | Danh sách ID học sinh cần thêm (ít nhất 1) |

```json
{
  "student_ids": ["uuid-1", "uuid-2"]
}
```

**Response 201 – Thêm thành công** (trả về danh sách học sinh trong lớp sau khi thêm)
```json
{
  "success": true,
  "message": "Thêm học sinh vào lớp thành công",
  "data": [ { "id": "uuid", "created_at": "...", "student": { "..." } } ]
}
```

**Response 404 – Học sinh không tồn tại**
```json
{ "success": false, "message": "Không tìm thấy học sinh phù hợp" }
```

**Response 409 – BR-008-01: Học sinh đã có lớp trong năm học**
```json
{ "success": false, "message": "Học sinh đã thuộc một lớp trong năm học này" }
```

**Response 409 – BR-008-02: Học sinh không đang học**
```json
{ "success": false, "message": "Chỉ thêm được học sinh có trạng thái \"Đang học\"" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "student_ids": ["Vui lòng chọn ít nhất 1 học sinh"]
  }
}
```

---

### DELETE /classes/:id/students/:student_id

Xóa một học sinh khỏi lớp.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |
| `student_id` | `string` (UUID) | ID học sinh |

**Response 200 – Xóa thành công**
```json
{
  "success": true,
  "message": "Xóa học sinh khỏi lớp thành công",
  "data": null
}
```

**Response 404 – Lớp không tồn tại**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

**Response 404 – Học sinh không thuộc lớp**
```json
{ "success": false, "message": "Học sinh không thuộc lớp này" }
```

---

## Subject Assignments

> **FR-009** – Phân công giáo viên bộ môn cho lớp học
> **Role yêu cầu:** `admin_school`
> **Business Rules:**
> - **BR-009-01:** Mỗi lớp chỉ có 1 giáo viên phụ trách cho mỗi môn học — `(class_id, subject_id)` unique (application-level)
> - Môn học phải thuộc cùng năm học với lớp
> - Giáo viên phải thuộc cùng trường với admin

---

### GET /classes/:id/assignments

Lấy danh sách phân công giáo viên bộ môn của lớp.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |

**Response 200**
```json
{
  "success": true,
  "message": "Lấy danh sách phân công thành công",
  "data": [
    {
      "id": "uuid",
      "created_at": "2025-09-01T00:00:00.000Z",
      "updated_at": "2025-09-01T00:00:00.000Z",
      "subject": { "id": "uuid", "name": "Toán" },
      "teacher": { "id": "uuid", "full_name": "Nguyễn Văn A", "teacher_code": "GV001" }
    }
  ]
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy lớp học phù hợp" }
```

---

### POST /classes/:id/assignments

Tạo phân công mới (gán giáo viên bộ môn cho một môn học trong lớp).

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `subject_id` | `string` (UUID) | Có | ID môn học (phải thuộc năm học của lớp) |
| `teacher_id` | `string` (UUID) | Có | ID giáo viên (phải thuộc trường) |

```json
{
  "subject_id": "uuid",
  "teacher_id": "uuid"
}
```

**Response 201 – Tạo thành công**
```json
{
  "success": true,
  "message": "Tạo phân công thành công",
  "data": {
    "id": "uuid",
    "subject": { "id": "uuid", "name": "Toán" },
    "teacher": { "id": "uuid", "full_name": "Nguyễn Văn A", "teacher_code": "GV001" }
  }
}
```

**Response 404 – Môn học không thuộc năm học của lớp**
```json
{ "success": false, "message": "Môn học không thuộc năm học của lớp này" }
```

**Response 404 – Giáo viên không tồn tại**
```json
{ "success": false, "message": "Không tìm thấy giáo viên phù hợp" }
```

**Response 409 – BR-009-01: Môn học đã có giáo viên phụ trách**
```json
{ "success": false, "message": "Môn học này đã có giáo viên phụ trách trong lớp" }
```

**Response 422 – Lỗi validation**
```json
{
  "success": false,
  "message": "Vui lòng kiểm tra lại thông tin",
  "errors": {
    "subject_id": ["Vui lòng chọn môn học"],
    "teacher_id": ["Vui lòng chọn giáo viên"]
  }
}
```

---

### PUT /classes/:id/assignments/:assignment_id

Cập nhật giáo viên phụ trách cho một phân công (thay đổi teacher).

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |
| `assignment_id` | `string` (UUID) | ID phân công |

**Request Body**

| Field | Type | Bắt buộc | Mô tả |
|-------|------|----------|-------|
| `teacher_id` | `string` (UUID) | Có | ID giáo viên mới |

```json
{ "teacher_id": "uuid" }
```

**Response 200 – Cập nhật thành công**
```json
{
  "success": true,
  "message": "Cập nhật phân công thành công",
  "data": { "id": "uuid", "subject": { "..." }, "teacher": { "..." } }
}
```

**Response 404 – Phân công không tồn tại**
```json
{ "success": false, "message": "Không tìm thấy phân công phù hợp" }
```

---

### DELETE /classes/:id/assignments/:assignment_id

Xóa một phân công giáo viên bộ môn.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `id` | `string` (UUID) | ID lớp học |
| `assignment_id` | `string` (UUID) | ID phân công |

**Response 200 – Xóa thành công**
```json
{
  "success": true,
  "message": "Xóa phân công thành công",
  "data": null
}
```

**Response 404 – Phân công không tồn tại**
```json
{ "success": false, "message": "Không tìm thấy phân công phù hợp" }
```

---

## Teacher Dashboard

> **Actor**: Giáo viên (role `teacher`)
> **Auth**: Bearer JWT (role `teacher`)
> **FR**: FR-010 đến FR-015

---

### GET /teacher/dashboard

**FR-010**: Lấy dashboard giáo viên — danh sách lớp chủ nhiệm và lớp bộ môn trong năm học đang diễn ra.

**BR-010-01**: Chỉ hiển thị lớp trong năm học có trạng thái `in_progress`.

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin dashboard thành công",
  "data": {
    "homeroom_classes": [
      {
        "id": "uuid",
        "name": "10A1",
        "academic_year": { "id": "uuid", "name": "2025-2026" },
        "_count": { "class_students": 35 }
      }
    ],
    "subject_assignments": [
      {
        "id": "uuid",
        "class": { "id": "uuid", "name": "10A2", "academic_year": { "id": "uuid", "name": "2025-2026" } },
        "subject": { "id": "uuid", "name": "Toán" }
      }
    ]
  }
}
```

---

### GET /teacher/homeroom/:class_id/students

**FR-012**: Lấy danh sách học sinh lớp chủ nhiệm kèm thông tin đầy đủ (bao gồm phụ huynh).

**BR-012-01**: GVCN chỉ xem được học sinh trong lớp mình chủ nhiệm.

**Path Parameters**

| Parameter | Type | Mô tả |
|-----------|------|-------|
| `class_id` | string UUID | ID lớp học |

**Response 200** — Mảng StudentInfo: id, student_code, full_name, date_of_birth, address, gender, study_status, user (id, email), parent (id, father_name, father_occupation, father_date_of_birth, mother_name, mother_occupation, mother_date_of_birth, phone, user).

**Response 403**
```json
{ "success": false, "message": "Bạn không phải giáo viên chủ nhiệm của lớp này" }
```

---

### GET /teacher/homeroom/:class_id/score-overview

**FR-011**: Bảng điểm tổng hợp (GVCN view) — ĐTB từng môn của tất cả học sinh.

**BR-011-01**: GVCN chỉ xem, không chỉnh sửa.
**BR-015-01**: ĐTB = Tổng(điểm × hệ số) / Tổng hệ số.

**Response 200**
```json
{
  "success": true,
  "message": "Lấy bảng điểm tổng hợp thành công",
  "data": {
    "class": { "id": "uuid", "name": "10A1", "academic_year": { "id": "uuid", "name": "2025-2026", "status": "in_progress" } },
    "subjects": [ { "id": "uuid", "name": "Toán" } ],
    "students": [
      { "id": "uuid", "full_name": "Nguyễn Văn A", "student_code": "TH012025001", "averages": { "subject-uuid": "8.50" } }
    ]
  }
}
```

---

### GET /teacher/homeroom/:class_id/students/:student_id/score-detail

**FR-011**: Bảng điểm chi tiết của một học sinh (tất cả môn, cột điểm, hệ số, ĐTB).

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin chi tiết thành công",
  "data": {
    "student": { "id": "uuid", "full_name": "Nguyễn Văn A", "student_code": "TH012025001" },
    "subjects": [
      {
        "subject": { "id": "uuid", "name": "Toán" },
        "columns": [ { "id": "uuid", "coefficient": 1, "order": 1, "value": "8.50" } ],
        "average": "8.50"
      }
    ]
  }
}
```

---

### GET /teacher/homeroom/:class_id/students/:student_id/comments

**FR-013**: Lấy danh sách nhận xét của học sinh (mới nhất trước — BR-013-07).

**Response 200** — Mảng Comment: id, content, scheduled_at, status (scheduled/sent), created_at, updated_at, student (id, full_name).

---

### POST /teacher/homeroom/:class_id/students/:student_id/comments

**FR-013**: Tạo nhận xét mới.

**BR-013-01**: Trạng thái mặc định `scheduled`.
**BR-013-09**: Không tạo khi năm học kết thúc.
**VR-013-03**: Thời gian gửi phải ở tương lai.

**Request Body**

| Trường | Type | Bắt buộc | Mô tả |
|--------|------|----------|-------|
| `content` | string | Có | Nội dung nhận xét |
| `scheduled_at` | string ISO 8601 | Có | Thời gian gửi (phải ở tương lai) |

**Response 201** — Comment object mới tạo.

**Response 400**
```json
{ "success": false, "message": "Năm học đã kết thúc, không thể thêm hoặc chỉnh sửa nhận xét" }
```

---

### PUT /teacher/homeroom/:class_id/comments/:comment_id

**FR-013**: Cập nhật nhận xét (chỉ khi `status = scheduled`).

**BR-013-02/06**: Chỉ chỉnh sửa khi còn `scheduled`; đã gửi thì không cho.

**Request Body** (tất cả optional): `content` (string), `scheduled_at` (ISO 8601, phải ở tương lai).

**Response 200** — Comment object đã cập nhật.

**Response 400**
```json
{ "success": false, "message": "Nhận xét đã gửi không thể chỉnh sửa" }
```

---

### GET /teacher/assignments/:assignment_id/scores

**FR-014**: Lấy bảng điểm chi tiết của phân công bộ môn.

**BR-014-01**: GVBM chỉ quản lý điểm lớp và môn mình được phân công.

**Response 200**
```json
{
  "success": true,
  "message": "Lấy bảng điểm thành công",
  "data": {
    "assignment": {
      "id": "uuid",
      "class": { "id": "uuid", "name": "10A1", "academic_year": { "id": "uuid", "name": "2025-2026", "status": "in_progress" } },
      "subject": { "id": "uuid", "name": "Toán" },
      "year_status": "in_progress"
    },
    "score_columns": [ { "id": "uuid", "coefficient": 1, "order": 1 } ],
    "students": [
      { "id": "uuid", "full_name": "Nguyễn Văn A", "student_code": "TH012025001", "scores": { "col-uuid": "8.50" }, "average": "8.50" }
    ]
  }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy phân công này" }
```

---

### POST /teacher/assignments/:assignment_id/score-columns

**FR-014**: Thêm cột điểm mới với hệ số.

**BR-014-05/06**: Chọn hệ số khi thêm; không thêm khi năm học kết thúc.

**Request Body**

| Trường | Type | Bắt buộc | Mô tả |
|--------|------|----------|-------|
| `coefficient` | number (int ≥ 1) | Có | Hệ số cột điểm |

**Response 201**
```json
{
  "success": true,
  "message": "Thêm cột điểm thành công",
  "data": { "id": "uuid", "coefficient": 2, "order": 3 }
}
```

---

### PUT /teacher/assignments/:assignment_id/scores

**FR-014 + FR-015**: Lưu điểm hàng loạt + tính lại ĐTB.

**BR-014-03/04**: Inline editing → nhấn "Lưu" → lưu + tính ĐTB.
**BR-015-01**: ĐTB = Tổng(điểm × hệ số) / Tổng hệ số (chỉ tính cột đã nhập, null được bỏ qua).
**BR-014-06**: Không lưu khi năm học đã kết thúc.

**Request Body**
```json
{
  "scores": [
    { "student_id": "uuid", "score_column_id": "uuid", "value": 8.5 },
    { "student_id": "uuid", "score_column_id": "uuid", "value": null }
  ]
}
```

**Response 200** — Bảng điểm đầy đủ sau khi cập nhật (cùng cấu trúc GET /scores).

**Response 400**
```json
{ "success": false, "message": "Năm học đã kết thúc, không thể chỉnh sửa điểm" }
```

---

## Portal — Phụ huynh & Học sinh

> **Actor**: `parent` (FR-016), `student` (FR-017)
> Tất cả endpoints yêu cầu JWT Bearer token với role tương ứng.
> Chỉ được **xem**, không được chỉnh sửa bất kỳ thông tin nào.

---

### GET /portal/parent

**FR-016** — Lấy danh sách con liên kết với tài khoản phụ huynh.

**BR-016-01**: Phụ huynh chỉ xem được thông tin của các con liên kết.
**BR-016-04/05**: FE dùng danh sách này để auto-select (1 con) hoặc hiển thị bộ chọn (nhiều con).

**Headers**
```
Authorization: Bearer <token>   (role: parent)
```

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin cổng phụ huynh thành công",
  "data": {
    "children": [
      {
        "id": "uuid",
        "student_code": "TH012025001",
        "full_name": "Nguyễn Văn A",
        "date_of_birth": "2010-01-15T00:00:00.000Z",
        "gender": "male",
        "study_status": "studying",
        "current_class": {
          "id": "uuid",
          "name": "10A1",
          "academic_year": { "id": "uuid", "name": "2025-2026", "status": "in_progress" }
        }
      }
    ]
  }
}
```

> `current_class` là lớp gần nhất của học sinh. Có thể `null` nếu chưa xếp lớp.

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy thông tin phụ huynh" }
```

---

### GET /portal/parent/students/:student_id

**FR-016** — Lấy bảng điểm và nhận xét của con được chọn.

**BR-016-01**: Nếu `student_id` không thuộc phụ huynh → 403.
**BR-016-03**: Chỉ được xem, không được chỉnh sửa.
**BR-016-02**: Chỉ trả về nhận xét đã gửi (`status = 'sent'`).

**Headers**
```
Authorization: Bearer <token>   (role: parent)
```

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin học sinh thành công",
  "data": {
    "student": {
      "id": "uuid",
      "student_code": "TH012025001",
      "full_name": "Nguyễn Văn A",
      "date_of_birth": "2010-01-15T00:00:00.000Z",
      "gender": "male",
      "study_status": "studying"
    },
    "scores": [
      {
        "class": {
          "id": "uuid",
          "name": "10A1",
          "academic_year": { "id": "uuid", "name": "2025-2026", "status": "in_progress" }
        },
        "subjects": [
          {
            "subject": { "id": "uuid", "name": "Toán" },
            "columns": [
              { "id": "uuid", "coefficient": 1, "order": 1, "value": "8.50" },
              { "id": "uuid", "coefficient": 2, "order": 2, "value": "9.00" }
            ],
            "average": "8.83"
          }
        ]
      }
    ],
    "comments": [
      {
        "id": "uuid",
        "content": "Học sinh có tinh thần học tập tốt.",
        "scheduled_at": "2025-10-01T07:00:00.000Z",
        "status": "sent",
        "created_at": "2025-09-25T10:00:00.000Z",
        "class": { "id": "uuid", "name": "10A1" },
        "teacher": { "id": "uuid", "full_name": "Trần Thị B" }
      }
    ]
  }
}
```

> `scores` sắp xếp theo năm học mới nhất trước. `average` là `null` nếu chưa có điểm nào.

**Response 403**
```json
{ "success": false, "message": "Học sinh này không thuộc tài khoản phụ huynh của bạn" }
```

---

### GET /portal/student

**FR-017** — Học sinh xem bảng điểm và nhận xét cá nhân.

**BR-017-01**: Chỉ xem được thông tin của chính mình.
**BR-017-03**: Chỉ được xem, không được chỉnh sửa.
**BR-017-02**: Chỉ trả về nhận xét đã gửi (`status = 'sent'`).

**Headers**
```
Authorization: Bearer <token>   (role: student)
```

**Response 200**
```json
{
  "success": true,
  "message": "Lấy thông tin cổng học sinh thành công",
  "data": {
    "student": {
      "id": "uuid",
      "student_code": "TH012025001",
      "full_name": "Nguyễn Văn A",
      "date_of_birth": "2010-01-15T00:00:00.000Z",
      "gender": "male",
      "study_status": "studying"
    },
    "scores": [
      {
        "class": {
          "id": "uuid",
          "name": "10A1",
          "academic_year": { "id": "uuid", "name": "2025-2026", "status": "in_progress" }
        },
        "subjects": [
          {
            "subject": { "id": "uuid", "name": "Toán" },
            "columns": [
              { "id": "uuid", "coefficient": 1, "order": 1, "value": "8.50" }
            ],
            "average": "8.50"
          }
        ]
      }
    ],
    "comments": [
      {
        "id": "uuid",
        "content": "Học sinh có tinh thần học tập tốt.",
        "scheduled_at": "2025-10-01T07:00:00.000Z",
        "status": "sent",
        "created_at": "2025-09-25T10:00:00.000Z",
        "class": { "id": "uuid", "name": "10A1" },
        "teacher": { "id": "uuid", "full_name": "Trần Thị B" }
      }
    ]
  }
}
```

**Response 404**
```json
{ "success": false, "message": "Không tìm thấy học sinh phù hợp" }
```
