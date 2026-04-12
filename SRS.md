# TÀI LIỆU SRS – HỆ THỐNG QUẢN LÝ ĐIỂM HỌC SINH

---

## Thông tin tài liệu

| Mục | Nội dung |
|---|---|
| Tên dự án | Hệ thống Quản lý điểm học sinh |
| Tài liệu tham chiếu | BRD v1.1 – Ngày 13/02/2026 |
| Người soạn | Senior Business Analyst |
| Ngày tạo | 13/02/2026 |
| Phiên bản | 1.1 |

---

## 1. Giới thiệu hệ thống

### 1.1 Mục đích tài liệu

Tài liệu SRS này mô tả chi tiết các yêu cầu chức năng và phi chức năng của Hệ thống Quản lý điểm học sinh, được phân tích từ tài liệu BRD v1.0. Tài liệu phục vụ cho đội ngũ phát triển (Dev), kiểm thử (QA) và quản lý dự án (PM) trong quá trình triển khai hệ thống.

### 1.2 Phạm vi hệ thống

Hệ thống bao gồm các module:

- Quản lý người dùng và phân quyền theo vai trò
- Quản lý master data giáo dục (trường học, năm học, môn học)
- Quản lý giáo viên, học sinh, phụ huynh
- Quản lý lớp học và phân công giảng dạy
- Quản lý điểm số và tính điểm trung bình
- Quản lý nhận xét học sinh (có lên lịch gửi tự động)
- Cổng thông tin cho phụ huynh và học sinh

### 1.3 Thuật ngữ và viết tắt

| Thuật ngữ | Giải thích |
|---|---|
| GVCN | Giáo viên chủ nhiệm |
| GVBM | Giáo viên bộ môn |
| CRUD | Create – Read – Update – Delete |
| CCCD | Căn cước công dân |
| BRD | Business Requirements Document |
| SRS | Software Requirements Specification |

---

## 2. Tổng quan hệ thống

### 2.1 Mô hình tổng quan

Hệ thống hoạt động theo mô hình phân quyền nhiều cấp:

- **Cấp hệ thống (Admin System):** Quản lý danh sách các trường học, tự động tạo tài khoản Admin School khi thêm trường mới.
- **Cấp trường (Admin School):** Quản lý toàn bộ dữ liệu trong phạm vi một trường: năm học, môn học, giáo viên, học sinh, lớp học, phân công giảng dạy.
- **Cấp giáo viên:** Quản lý điểm số (GVBM) và nhận xét học sinh (GVCN).
- **Cấp phụ huynh / học sinh:** Xem điểm và nhận xét.

### 2.2 Luồng nghiệp vụ chính

```
Admin System tạo trường → Tự động tạo Admin School
    → Admin School thiết lập năm học, môn học
    → Admin School thêm giáo viên, học sinh
    → Admin School tạo lớp, gán GVCN, thêm học sinh vào lớp
    → Admin School phân công GVBM cho lớp
    → GVBM nhập điểm, thêm cột điểm → Hệ thống tính ĐTB
    → GVCN xem bảng điểm, tạo nhận xét → Hệ thống tự động gửi nhận xét
    → Phụ huynh / Học sinh xem điểm và nhận xét
```

---

## 3. Actors & Vai trò

| STT | Actor | Vai trò | Phạm vi truy cập |
|---|---|---|---|
| 1 | Admin System | Quản lý danh sách trường học trên toàn hệ thống | Toàn hệ thống |
| 2 | Admin School | Quản lý toàn bộ dữ liệu trong phạm vi trường được gán | Phạm vi trường |
| 3 | Giáo viên | Quản lý điểm (với vai trò GVBM), quản lý nhận xét (với vai trò GVCN), xem thông tin lớp | Lớp được phân công |
| 4 | Phụ huynh | Xem điểm và nhận xét của con | Chỉ thông tin con mình |
| 5 | Học sinh | Xem điểm và nhận xét cá nhân | Chỉ thông tin cá nhân |

---

## 4. Danh sách chức năng (Functional Requirements)

---

### FR-001: Đăng nhập hệ thống

**Mô tả chức năng:**
Hệ thống cho phép người dùng đăng nhập bằng email và mật khẩu. Sau khi đăng nhập thành công, hệ thống phân quyền hiển thị giao diện và chức năng tương ứng với role của người dùng.

**Actor:** Admin System, Admin School, Giáo viên, Phụ huynh, Học sinh

**Business Flow:**
1. Người dùng truy cập trang đăng nhập
2. Người dùng nhập email và mật khẩu
3. Hệ thống xác thực thông tin đăng nhập
4. Nếu hợp lệ → chuyển đến giao diện tương ứng với role
5. Nếu không hợp lệ → hiển thị thông báo lỗi

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Email | String | Có | Email đăng nhập |
| Mật khẩu | String | Có | Mật khẩu tài khoản |

**Output Data:**
- Đăng nhập thành công: Chuyển hướng đến dashboard tương ứng với role
- Đăng nhập thất bại: Thông báo lỗi

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-001-01 | Email không được để trống | Bắt buộc nhập |
| VR-001-02 | Mật khẩu không được để trống | Bắt buộc nhập |
| VR-001-03 | Email phải đúng định dạng | Kiểm tra format email |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-001-01 | "Vui lòng nhập email" | Email để trống |
| MSG-001-02 | "Vui lòng nhập mật khẩu" | Mật khẩu để trống |
| MSG-001-03 | "Email hoặc mật khẩu không đúng" | Thông tin đăng nhập không hợp lệ |
| MSG-001-04 | "Đăng nhập thành công" | Xác thực thành công |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-001-01 | Hệ thống phân quyền chức năng dựa trên role của tài khoản |
| BR-001-02 | Mỗi tài khoản được gắn với đúng 1 role |
| BR-001-03 | Tài khoản Admin System mặc định: Email: adminstpoint@gmail.com, Password: adminstpoiNt1122@ |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-001-01 | Tài khoản không tồn tại | Hiển thị MSG-001-03 |
| EX-001-02 | Mật khẩu sai | Hiển thị MSG-001-03 |
| EX-001-03 | Lỗi hệ thống / mất kết nối | BRD không đề cập |

---

### FR-002: Quản lý trường học (CRUD)

**Mô tả chức năng:**
Admin System quản lý danh sách trường học, bao gồm: tạo mới, xem, sửa, xóa, tìm kiếm. Khi tạo trường học mới, hệ thống tự động tạo tài khoản Admin School tương ứng.

**Actor:** Admin System

**Business Flow:**

*Tạo trường học:*
1. Admin System chọn chức năng tạo trường học
2. Nhập thông tin trường học
3. Hệ thống validate dữ liệu
4. Hệ thống lưu thông tin trường học
5. Hệ thống tự động tạo tài khoản Admin School theo trường học đó
6. Hiển thị thông báo thành công

*Xem danh sách trường học:*
1. Admin System truy cập trang quản lý trường học
2. Hệ thống hiển thị danh sách trường học

*Tìm kiếm trường học:*
1. Admin System nhập từ khóa vào ô tìm kiếm
2. Hệ thống tìm kiếm theo tên trường hoặc mã trường (không phân biệt hoa thường)
3. Hệ thống hiển thị danh sách kết quả phù hợp
4. Nếu không có kết quả → hiển thị thông báo "Không tìm thấy trường học phù hợp"

*Sửa trường học:*
1. Admin System chọn trường học cần sửa
2. Cập nhật thông tin
3. Hệ thống validate và lưu

*Xóa trường học:*
1. Admin System chọn trường học cần xóa
2. Hệ thống kiểm tra trường có dữ liệu con liên quan (giáo viên, học sinh) hay không
3. Nếu có dữ liệu con → hiển thị cảnh báo MSG-002-06, không thực hiện xóa
4. Nếu không có dữ liệu con → hiển thị xác nhận xóa
5. Admin System xác nhận → hệ thống xóa trường học

**Input Data:**

*Form tạo / sửa trường học:*

| Trường | Kiểu dữ liệu | Bắt buộc | Unique | Mô tả |
|---|---|---|---|---|
| Tên trường | String | Có | Không | Tên trường học |
| Mã trường | String | Có | Có | Mã trường học, Admin System có thể tự nhập. Tối đa 8 ký tự, chỉ bao gồm chữ và số |

*Tìm kiếm:*

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Từ khóa | String | Không | Tìm theo tên trường hoặc mã trường. Không phân biệt hoa thường |

**Output Data:**
- Danh sách trường học (toàn bộ hoặc theo kết quả tìm kiếm)
- Tài khoản Admin School được tạo tự động khi thêm trường mới

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-002-01 | Tên trường không được để trống | Bắt buộc |
| VR-002-02 | Mã trường không được để trống | Bắt buộc |
| VR-002-03 | Mã trường phải duy nhất trong hệ thống | Unique constraint |
| VR-002-04 | Mã trường tối đa 8 ký tự | Giới hạn độ dài |
| VR-002-05 | Mã trường chỉ bao gồm chữ cái và chữ số (không có ký tự đặc biệt, khoảng trắng) | Format constraint |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-002-01 | "Vui lòng nhập tên trường" | Tên trường để trống |
| MSG-002-02 | "Vui lòng nhập mã trường" | Mã trường để trống |
| MSG-002-03 | "Mã trường đã tồn tại trong hệ thống" | Mã trường trùng |
| MSG-002-04 | "Mã trường không được vượt quá 8 ký tự" | Mã trường quá 8 ký tự |
| MSG-002-05 | "Mã trường chỉ được chứa chữ cái và chữ số" | Mã trường có ký tự đặc biệt hoặc khoảng trắng |
| MSG-002-06 | "Không thể xóa trường học này vì đang có giáo viên hoặc học sinh liên quan" | Trường có dữ liệu con (giáo viên, học sinh) |
| MSG-002-07 | "Không tìm thấy trường học phù hợp" | Từ khóa không khớp với bất kỳ trường nào |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-002-01 | Khi tạo trường học, hệ thống tự động tạo tài khoản Admin School theo trường học đó |
| BR-002-02 | Mã trường là duy nhất (unique) trên toàn hệ thống |
| BR-002-03 | Mã trường do Admin System tự nhập |
| BR-002-04 | Mã trường tối đa 8 ký tự, chỉ bao gồm chữ cái và chữ số (regex: `^[a-zA-Z0-9]{1,8}$`) |
| BR-002-05 | Không được xóa trường học nếu trường đó còn giáo viên hoặc học sinh liên quan |
| BR-002-06 | Tìm kiếm theo tên trường hoặc mã trường, không phân biệt hoa thường (case-insensitive) |
| BR-002-07 | Ô tìm kiếm để trống → hiển thị toàn bộ danh sách trường học |

> **Lưu ý:** BRD không đề cập chi tiết thông tin tài khoản Admin School được tạo tự động (email, password mặc định). Thông tin đăng nhập adminstpoint@gmail.com / adminstpoiNt1122@ là tài khoản của Admin System, không phải Admin School. **Cần làm rõ cơ chế tạo tài khoản Admin School với stakeholder.**

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-002-01 | Mã trường trùng | Hiển thị MSG-002-03, không cho phép lưu |
| EX-002-03 | Mã trường quá 8 ký tự | Hiển thị MSG-002-04, không cho phép lưu |
| EX-002-04 | Mã trường chứa ký tự không hợp lệ | Hiển thị MSG-002-05, không cho phép lưu |
| EX-002-05 | Xóa trường học đang có giáo viên hoặc học sinh | Hiển thị MSG-002-06, không thực hiện xóa |
| EX-002-02 | Không thể tạo tài khoản Admin School | BRD không đề cập |

---

### FR-003: Quản lý năm học

**Mô tả chức năng:**
Admin School quản lý danh sách năm học trong phạm vi trường, bao gồm: tạo, xem, sửa năm học. Mỗi năm học có trạng thái theo vòng đời: Chuẩn bị diễn ra → Đang diễn ra → Đã kết thúc.

**Actor:** Admin School

**Business Flow:**
1. Admin School truy cập chức năng quản lý năm học
2. Chọn tạo năm học mới
3. Nhập thông tin: tên, thời gian bắt đầu, thời gian kết thúc
4. Hệ thống validate dữ liệu
5. Hệ thống lưu năm học với trạng thái mặc định
6. Hiển thị thông báo thành công

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Tên năm học | String | Có | Tên hiển thị của năm học (VD: 2025-2026) |
| Thời gian bắt đầu | Date | Có | Ngày bắt đầu năm học |
| Thời gian kết thúc | Date | Có | Ngày kết thúc năm học |
| Trạng thái | Enum | Hệ thống quản lý | Chuẩn bị diễn ra / Đang diễn ra / Đã kết thúc |

**Output Data:**
- Danh sách năm học (có trạng thái)
- Chi tiết năm học

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-003-01 | Tên năm học không được để trống | Bắt buộc |
| VR-003-02 | Thời gian bắt đầu không được để trống | Bắt buộc |
| VR-003-03 | Thời gian kết thúc không được để trống | Bắt buộc |
| VR-003-04 | Thời gian kết thúc > Thời gian bắt đầu | Logic thời gian |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-003-01 | "Vui lòng nhập tên năm học" | Tên để trống |
| MSG-003-02 | "Vui lòng chọn thời gian bắt đầu" | Thiếu thời gian bắt đầu |
| MSG-003-03 | "Vui lòng chọn thời gian kết thúc" | Thiếu thời gian kết thúc |
| MSG-003-04 | "Thời gian kết thúc phải sau thời gian bắt đầu" | Kết thúc <= Bắt đầu |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-003-01 | Tại một thời điểm, chỉ có tối đa 1 năm học ở trạng thái "Đang diễn ra" |
| BR-003-02 | Trạng thái năm học gồm 3 giá trị: "Chuẩn bị diễn ra", "Đang diễn ra", "Đã kết thúc" |
| BR-003-03 | Khi năm học ở trạng thái "Đã kết thúc": toàn bộ điểm và nhận xét thuộc năm học đó bị khóa – không được thêm mới hoặc chỉnh sửa |

> **Lưu ý:** BRD không đề cập cơ chế chuyển trạng thái năm học (tự động theo thời gian hay Admin School thao tác thủ công). **BRD không đề cập.**

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-003-01 | Tạo năm học khi đã có năm học "Đang diễn ra" | Hệ thống không cho phép tạo thêm năm học trạng thái "Đang diễn ra" |
| EX-003-02 | Trùng tên năm học | BRD không đề cập |

---

### FR-004: Quản lý môn học

**Mô tả chức năng:**
Admin School quản lý danh sách môn học theo từng năm học. Mỗi năm học có danh sách môn học riêng.

**Actor:** Admin School

**Business Flow:**

*Thêm / sửa / xóa môn học:*
1. Admin School chọn năm học
2. Truy cập chức năng quản lý môn học
3. Thêm / sửa / xóa môn học cho năm học đó
4. Hệ thống validate và lưu

*Sao chép danh sách môn học:*
1. Admin School đang xem danh sách môn học của một năm học
2. Nhấn nút "Sao chép danh sách môn học"
3. Hệ thống hiển thị popup danh sách năm học đang diễn ra và sắp diễn ra (trừ năm đang xem)
4. Admin School chọn năm học đích
5. Nếu năm học đích đã có môn học → hiển thị cảnh báo "Năm học này đã có dữ liệu, có thay đổi nữa hay không?"
   - Chọn "Có": ghi đè toàn bộ danh sách môn học ở năm đích
   - Chọn "Không": hủy thao tác
6. Nếu năm học đích chưa có môn học → thực hiện sao chép ngay
7. Hiển thị thông báo "Tạo dữ liệu thành công cho năm học {tên năm học}"

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Tên môn học | String | Có | Tên môn học |
| Năm học | Reference | Có | Liên kết đến năm học |

**Output Data:**
- Danh sách môn học theo năm học

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-004-01 | Tên môn học không được để trống | Bắt buộc |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-004-01 | "Vui lòng nhập tên môn học" | Tên để trống |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-004-01 | Mỗi năm học có danh sách môn học riêng biệt |
| BR-004-01b | Không thể thêm môn học vào năm học có trạng thái `completed` |
| BR-004-02 | Tên môn học phải unique trong cùng năm học (case-insensitive) |
| BR-004-03 | Không thể xóa môn học đã có phân công giảng dạy |
| BR-004-04 | Sao chép danh sách môn học: chỉ cho phép sang năm học `in_progress` hoặc `preparing`; nếu năm đích đã có phân công giảng dạy thì không thể ghi đè |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-004-01 | Xóa môn học đã được phân công cho lớp | Hiển thị lỗi, không cho xóa (BR-004-03) |
| EX-004-02 | Trùng tên môn học trong cùng năm học | Hiển thị lỗi, không cho tạo |
| EX-004-03 | Sao chép sang năm đích đã có phân công | Hiển thị lỗi, không thực hiện ghi đè (BR-004-04) |

---

### FR-005: Quản lý giáo viên

**Mô tả chức năng:**
Admin School quản lý danh sách giáo viên trong trường, bao gồm thêm mới, xem, sửa thông tin giáo viên. Mỗi giáo viên có tài khoản đăng nhập hệ thống.

**Actor:** Admin School

**Business Flow:**

*Thêm giáo viên:*
1. Admin School chọn chức năng thêm giáo viên
2. Nhập đầy đủ thông tin giáo viên (không có trường mật khẩu)
3. Hệ thống validate dữ liệu (kiểm tra unique CCCD, mã số giáo viên, email)
4. Hệ thống tự động tạo mật khẩu tạm thời ngẫu nhiên
5. Hệ thống tạo tài khoản giáo viên và gửi email chứa mật khẩu tạm thời đến email giáo viên
6. Lưu thông tin và hiển thị thông báo thành công

*Xem danh sách giáo viên:*
1. Admin School truy cập danh sách giáo viên
2. Hệ thống hiển thị danh sách

*Sửa giáo viên:*
1. Admin School chọn giáo viên cần sửa
2. Cập nhật thông tin
3. Hệ thống validate và lưu

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Unique | Mô tả |
|---|---|---|---|---|
| Họ tên | String | Có | Không | Họ và tên giáo viên |
| Ngày sinh | Date | Có | Không | Ngày sinh |
| Địa chỉ | String | Có | Không | Địa chỉ giáo viên |
| CCCD | String | Có | Có | Số căn cước công dân |
| Mã số giáo viên | String | Có | Có | Mã định danh giáo viên |
| Giới tính | Enum | Có | Không | Giới tính |
| Trạng thái làm việc | Enum | Có | Không | Đang làm việc / Đã nghỉ |
| Email | String (Email) | Có | Không | Email đăng nhập — hệ thống tự gửi mật khẩu tạm thời đến địa chỉ này |

**Output Data:**
- Danh sách giáo viên (có trạng thái)
- Chi tiết thông tin giáo viên

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-005-01 | Họ tên không được để trống | Bắt buộc |
| VR-005-02 | Ngày sinh không được để trống | Bắt buộc |
| VR-005-03 | Địa chỉ không được để trống | Bắt buộc |
| VR-005-04 | CCCD không được để trống | Bắt buộc |
| VR-005-05 | CCCD phải duy nhất trong hệ thống | Unique constraint |
| VR-005-06 | Mã số giáo viên không được để trống | Bắt buộc |
| VR-005-07 | Mã số giáo viên phải duy nhất trong hệ thống | Unique constraint |
| VR-005-08 | Email không được để trống và đúng định dạng | Bắt buộc |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-005-01 | "Vui lòng nhập họ tên" | Họ tên để trống |
| MSG-005-02 | "Vui lòng nhập ngày sinh" | Ngày sinh để trống |
| MSG-005-03 | "Vui lòng nhập địa chỉ" | Địa chỉ để trống |
| MSG-005-04 | "Vui lòng nhập số CCCD" | CCCD để trống |
| MSG-005-05 | "Số CCCD đã tồn tại trong hệ thống" | CCCD trùng |
| MSG-005-06 | "Vui lòng nhập mã số giáo viên" | Mã số để trống |
| MSG-005-07 | "Mã số giáo viên đã tồn tại trong hệ thống" | Mã số trùng |
| MSG-005-08 | "Vui lòng nhập email" | Email để trống |
| MSG-005-09 | "Email không đúng định dạng" | Email sai format |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-005-01 | CCCD phải là duy nhất (unique) trên toàn hệ thống |
| BR-005-02 | Mã số giáo viên phải là duy nhất (unique) trên toàn hệ thống |
| BR-005-03 | Giáo viên được tạo tài khoản đăng nhập bằng email; hệ thống tự sinh mật khẩu tạm thời và gửi qua email |
| BR-005-04 | Admin School không nhập mật khẩu khi tạo giáo viên — mật khẩu tạm thời được tạo tự động (12 ký tự ngẫu nhiên) |

> **Lưu ý:** BRD không đề cập các giá trị cụ thể của "Giới tính". Cũng không đề cập chức năng xóa giáo viên. **BRD không đề cập.**

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-005-01 | CCCD trùng | Hiển thị MSG-005-04, không cho phép lưu |
| EX-005-02 | Mã số giáo viên trùng | Hiển thị MSG-005-06, không cho phép lưu |
| EX-005-03 | Xóa giáo viên đang được phân công lớp | BRD không đề cập |

---

### FR-006: Quản lý học sinh

**Mô tả chức năng:**
Admin School quản lý danh sách học sinh trong trường. Thông tin học sinh bao gồm cả thông tin phụ huynh (cha, mẹ). Khi tạo học sinh, hệ thống kiểm tra email phụ huynh để quyết định tạo mới hay liên kết với tài khoản phụ huynh đã có. Cả học sinh và phụ huynh (nếu mới) đều nhận email thiết lập mật khẩu sau khi tạo.

**Actor:** Admin School

**Business Flow:**

*Thêm học sinh – Email phụ huynh chưa tồn tại:*
1. Admin School chọn chức năng thêm học sinh
2. Nhập thông tin học sinh và thông tin phụ huynh (không có trường mật khẩu)
3. Hệ thống validate dữ liệu
4. Hệ thống kiểm tra email phụ huynh → không tìm thấy trong hệ thống
5. Hệ thống tạo tài khoản học sinh (chưa có mật khẩu) và tài khoản phụ huynh (chưa có mật khẩu)
6. Hệ thống gửi email thiết lập mật khẩu đến email học sinh
7. Hệ thống gửi email thiết lập mật khẩu đến email phụ huynh
8. Lưu thông tin và hiển thị thông báo thành công

*Thêm học sinh – Email phụ huynh đã tồn tại:*
1. Admin School chọn chức năng thêm học sinh
2. Nhập thông tin học sinh và thông tin phụ huynh (không có trường mật khẩu)
3. Hệ thống validate dữ liệu
4. Hệ thống kiểm tra email phụ huynh → tìm thấy tài khoản phụ huynh đã có
5. Hệ thống tạo tài khoản học sinh (chưa có mật khẩu), liên kết học sinh với tài khoản phụ huynh đã có (không tạo mới)
6. Hệ thống gửi email thiết lập mật khẩu đến email học sinh (không gửi email cho phụ huynh)
7. Lưu thông tin và hiển thị thông báo thành công

*Xem / Sửa học sinh:*
1. Admin School chọn học sinh trong danh sách
2. Xem hoặc chỉnh sửa thông tin
3. Hệ thống validate và lưu

**Input Data:**

*Thông tin học sinh:*

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| ID | String/Auto | Hệ thống tự tạo | Mã định danh học sinh. Format: `[Mã trường][Năm nhập học][Số thứ tự]`. Ví dụ: `TH012025001`. Số thứ tự tăng dần, mỗi năm reset lại từ 1 |
| Họ tên | String | Có | Họ và tên học sinh |
| Ngày sinh | Date | Có | Ngày sinh |
| Địa chỉ | String | Có | Địa chỉ |
| Giới tính | Enum | Có | Giới tính |
| Email đăng nhập | String (Email) | Có | Email để đăng nhập hệ thống. Sau khi tạo, hệ thống gửi email thiết lập mật khẩu |
| Trạng thái học | Enum | Có | Đang học / Đã nghỉ |

*Thông tin phụ huynh:*

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Họ tên cha | String | Có | Họ tên người cha |
| Nghề nghiệp cha | String | Có | Nghề nghiệp người cha |
| Ngày sinh cha | Date | Có | Ngày sinh người cha |
| Họ tên mẹ | String | Có | Họ tên người mẹ |
| Nghề nghiệp mẹ | String | Có | Nghề nghiệp người mẹ |
| Ngày sinh mẹ | Date | Có | Ngày sinh người mẹ |
| Số điện thoại mẹ | String | Có | Số điện thoại người mẹ |
| Email đăng nhập | String (Email) | Có | Email để phụ huynh đăng nhập. Hệ thống tự tra cứu: nếu chưa tồn tại → tạo mới; nếu đã tồn tại → liên kết |
| Số điện thoại | String | Có | Số điện thoại liên lạc chung |

> **Lưu ý:** Không rõ "Bắt buộc" hay "Không bắt buộc" cho từng trường. Các trường trên được suy luận là bắt buộc do BRD liệt kê chúng. **Cần xác nhận lại với stakeholder.**

**Output Data:**
- Danh sách học sinh (có trạng thái)
- Chi tiết thông tin học sinh kèm thông tin phụ huynh

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-006-01 | Họ tên học sinh không được để trống | Bắt buộc |
| VR-006-02 | Ngày sinh không được để trống | Bắt buộc |
| VR-006-03 | Email đăng nhập học sinh không được để trống, đúng format | Bắt buộc |
| VR-006-04 | Thông tin phụ huynh đầy đủ | Bắt buộc |
| VR-006-05 | Email đăng nhập phụ huynh không được để trống, đúng format | Bắt buộc |
| VR-006-06 | Số điện thoại phụ huynh không được để trống | Bắt buộc |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-006-01 | "Vui lòng nhập họ tên học sinh" | Họ tên để trống |
| MSG-006-02 | "Vui lòng nhập ngày sinh" | Ngày sinh để trống |
| MSG-006-03 | "Vui lòng nhập email đăng nhập" | Email để trống |
| MSG-006-04 | "Email không đúng định dạng" | Email sai format |
| MSG-006-05 | "Vui lòng nhập thông tin phụ huynh" | Thiếu thông tin phụ huynh |
| MSG-006-06 | "Vui lòng nhập email phụ huynh" | Email phụ huynh để trống |
| MSG-006-07 | "Email phụ huynh không đúng định dạng" | Email phụ huynh sai format |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-006-01 | Mỗi học sinh có ID duy nhất do hệ thống tự tạo theo format: `[Mã trường] + [Năm nhập học] + [Số thứ tự tăng dần]`. Năm nhập học = năm hiện tại khi tạo học sinh. Số thứ tự reset lại từ 1 mỗi năm |
| BR-006-02 | Thông tin phụ huynh được lưu kèm theo thông tin học sinh |
| BR-006-03 | Phụ huynh có tài khoản riêng để đăng nhập hệ thống |
| BR-006-04 | Học sinh có tài khoản riêng để đăng nhập hệ thống |
| BR-006-05 | Khi tạo học sinh, hệ thống kiểm tra email phụ huynh: nếu chưa tồn tại → tạo mới tài khoản phụ huynh; nếu đã tồn tại → liên kết học sinh với tài khoản phụ huynh hiện có, không tạo thêm |
| BR-006-06 | Sau khi tạo học sinh: luôn gửi email thiết lập mật khẩu đến email học sinh. Chỉ gửi email thiết lập mật khẩu đến email phụ huynh nếu tài khoản phụ huynh vừa được tạo mới |
| BR-006-07 | 1 tài khoản phụ huynh có thể liên kết với nhiều học sinh (nhiều con) |

> **Lưu ý:** Trạng thái học gồm 2 giá trị: "Đang học" và "Đã nghỉ".

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-006-01 | Email học sinh đã tồn tại trong hệ thống | BRD không đề cập cách xử lý |
| EX-006-02 | Email phụ huynh đã tồn tại → liên kết học sinh với tài khoản phụ huynh hiện có (BR-006-05), không báo lỗi |
| EX-006-03 | Xóa học sinh đang thuộc lớp | BRD không đề cập |

---

### FR-007: Quản lý lớp học

**Mô tả chức năng:**
Admin School quản lý danh sách lớp học theo năm học. Mỗi lớp có một giáo viên chủ nhiệm. Tên lớp không được trùng trong cùng năm học.

**Actor:** Admin School

**Business Flow:**
1. Admin School chọn năm học
2. Truy cập chức năng quản lý lớp học
3. Tạo lớp mới: nhập tên lớp, chọn GVCN
4. Hệ thống validate: kiểm tra trùng tên, kiểm tra GVCN đã được gán lớp khác chưa
5. Hệ thống lưu và hiển thị thông báo thành công

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Tên lớp | String | Có | Tên lớp học (VD: 10A1) |
| Năm học | Reference | Có | Liên kết đến năm học |
| Giáo viên chủ nhiệm | Reference | Có | Chọn từ danh sách giáo viên khả dụng |

**Output Data:**
- Danh sách lớp học theo năm học
- Chi tiết lớp học (tên lớp, GVCN, danh sách học sinh)

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-007-01 | Tên lớp không được để trống | Bắt buộc |
| VR-007-02 | Tên lớp không được trùng trong cùng năm học | Unique trong scope năm học |
| VR-007-03 | Phải chọn giáo viên chủ nhiệm | Bắt buộc |
| VR-007-04 | Giáo viên đã là GVCN lớp khác trong cùng năm học → không được chọn | Constraint |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-007-01 | "Vui lòng nhập tên lớp" | Tên lớp để trống |
| MSG-007-02 | "Tên lớp đã tồn tại trong năm học này" | Trùng tên lớp |
| MSG-007-03 | "Vui lòng chọn giáo viên chủ nhiệm" | Chưa chọn GVCN |
| MSG-007-04 | "Giáo viên này đã là chủ nhiệm lớp khác" | GVCN đã được gán |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-007-01 | Không trùng tên lớp trong cùng năm học |
| BR-007-02 | 1 giáo viên chỉ chủ nhiệm tối đa 1 lớp trong cùng năm học |
| BR-007-03 | Giáo viên đã là GVCN sẽ không hiển thị trong danh sách chọn khi tạo/sửa lớp khác |
| BR-007-04 | Chỉ được tạo lớp học trong năm học có trạng thái `in_progress` (đang diễn ra) hoặc `preparing` (sắp diễn ra). Không thể tạo lớp trong năm học `completed` |
| BR-007-05 | Danh sách lớp học mặc định lọc theo năm học đang diễn ra (`in_progress`) khi vào trang |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-007-01 | Trùng tên lớp | Hiển thị thông báo lỗi (MSG-007-02), tạo lớp không thành công |
| EX-007-02 | GVCN đã được gán lớp khác | Không hiển thị trong dropdown (BR-007-03) |
| EX-007-03 | Xóa lớp đã có học sinh | BRD không đề cập |

---

### FR-008: Quản lý danh sách học sinh trong lớp

**Mô tả chức năng:**
Trong chi tiết lớp học, có mục danh sách học sinh. Admin School thêm học sinh vào lớp thông qua popup với checkbox. Chỉ những học sinh có trạng thái "Đang học" mới được hiển thị. Mỗi học sinh chỉ thuộc 1 lớp trong cùng năm học.

**Actor:** Admin School

**Business Flow:**
1. Admin School truy cập chi tiết lớp học, xem mục danh sách học sinh
2. Nhấn nút "Chỉnh sửa danh sách học sinh"
3. Hệ thống hiển thị popup danh sách học sinh khả dụng (trạng thái "Đang học", chưa thuộc lớp nào trong năm học)
4. Admin School có thể tìm kiếm học sinh theo mã học sinh hoặc họ tên thông qua ô tìm kiếm phía trên danh sách (lọc phía client, không cần gọi API)
5. Admin School tick checkbox chọn học sinh cần thêm
6. Nhấn nút "Thêm vào lớp"
7. Hệ thống lưu và cập nhật danh sách

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Lớp học | Reference | Có | Lớp cần thêm học sinh |
| Học sinh | Reference (multi-select) | Có | Chọn từ popup danh sách học sinh khả dụng |

**Output Data:**
- Danh sách học sinh trong lớp (cập nhật)

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-008-01 | Chỉ hiển thị học sinh trạng thái "Đang học" | Lọc theo trạng thái |
| VR-008-02 | Học sinh đã thuộc lớp khác trong năm học → không hiển thị | Lọc theo ràng buộc |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-008-01 | BRD không đề cập cụ thể | - |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-008-01 | 1 học sinh chỉ thuộc 1 lớp trong cùng năm học |
| BR-008-02 | Chỉ hiển thị học sinh có trạng thái "Đang học" trong popup |
| BR-008-03 | Thêm học sinh thông qua giao diện popup với checkbox chọn nhiều, sau đó nhấn nút "Thêm vào lớp" |
| BR-008-04 | Popup có ô tìm kiếm để lọc học sinh theo mã học sinh hoặc họ tên (client-side, không cần gọi API thêm) |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-008-01 | Học sinh đã thuộc lớp khác trong năm học | Không hiển thị trong popup (BR-008-02) |
| EX-008-02 | Xóa học sinh khỏi lớp | BRD không đề cập |

---

### FR-009: Phân công giáo viên bộ môn

**Mô tả chức năng:**
Trong chi tiết lớp học, có nút "Thông tin giáo viên bộ môn". Khi click, hiển thị trang mới với danh sách phân công dạng bảng 2 cột (Tên môn học – Giáo viên phụ trách). Admin School chọn môn và giáo viên qua droplist.

**Actor:** Admin School

**Business Flow:**
1. Admin School truy cập chi tiết lớp học
2. Nhấn nút "Thông tin giáo viên bộ môn"
3. Hệ thống hiển thị trang mới với danh sách phân công gồm 2 cột: Tên môn học và Giáo viên phụ trách
4. Tại cột Tên môn học: droplist hiển thị danh sách môn học của năm học đó
5. Tại cột Giáo viên: droplist hiển thị danh sách giáo viên để chọn
6. Khi cần thêm dòng phân công mới, nhấn nút "Thêm mới"
7. Hệ thống lưu phân công

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Lớp học | Reference | Có | Lớp được phân công |
| Môn học | Reference | Có | Môn học cần thêm |
| Giáo viên | Reference | Có | Giáo viên phụ trách |

**Output Data:**
- Danh sách phân công giáo viên bộ môn của lớp (Môn – Giáo viên)

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-009-01 | Phải chọn môn học | Bắt buộc |
| VR-009-02 | Phải chọn giáo viên | Bắt buộc |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-009-01 | "Vui lòng chọn môn học" | Chưa chọn môn |
| MSG-009-02 | "Vui lòng chọn giáo viên" | Chưa chọn giáo viên |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-009-01 | Mỗi lớp có danh sách môn học và giáo viên phụ trách tương ứng |
| BR-009-02 | Giao diện hiển thị dạng danh sách 2 cột với droplist chọn môn và giáo viên |
| BR-009-03 | Có nút "Thêm mới" để thêm dòng phân công |

> **Lưu ý:** BRD không đề cập các ràng buộc: 1 giáo viên dạy tối đa bao nhiêu lớp, 1 môn trong 1 lớp có tối đa mấy giáo viên, giáo viên có phải dạy đúng chuyên môn không. **BRD không đề cập.**

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-009-01 | Trùng phân công (cùng lớp, cùng môn) | BRD không đề cập |
| EX-009-02 | Xóa phân công khi đã có điểm | BRD không đề cập |

---

### FR-010: Dashboard giáo viên

**Mô tả chức năng:**
Giáo viên sau khi đăng nhập sẽ thấy dashboard hiển thị danh sách lớp chủ nhiệm và lớp bộ môn được phân công.

**Actor:** Giáo viên

**Business Flow:**
1. Giáo viên đăng nhập hệ thống
2. Hệ thống hiển thị dashboard gồm:
   - Danh sách lớp chủ nhiệm (nếu có)
   - Danh sách lớp bộ môn được phân công

**Input Data:**
- Không có input từ người dùng (hệ thống tự động load theo tài khoản đăng nhập)

**Output Data:**
- Danh sách lớp chủ nhiệm
- Danh sách lớp bộ môn (kèm tên môn)

**Validation Rules:**
- Không áp dụng

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-010-01 | Dashboard chỉ hiển thị lớp trong năm học hiện tại (trạng thái "Đang diễn ra") |
| BR-010-02 | Phân tách rõ ràng giữa lớp chủ nhiệm và lớp bộ môn |

> **Lưu ý:** BR-010-01 được suy luận logic từ ngữ cảnh. BRD không đề cập rõ ràng dashboard hiển thị theo năm học nào.

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-010-01 | Giáo viên chưa được phân công lớp nào | Hiển thị dashboard trống / thông báo chưa có lớp |

---

### FR-011: Lớp chủ nhiệm – Xem bảng điểm

**Mô tả chức năng:**
Giáo viên chủ nhiệm xem bảng điểm trung bình tất cả các môn của học sinh trong lớp. Ngoài ra có thể click vào từng học sinh để xem bảng điểm chi tiết.

**Actor:** Giáo viên (vai trò GVCN)

**Business Flow:**

*Xem bảng điểm trung bình:*
1. Giáo viên chọn lớp chủ nhiệm từ dashboard
2. Hệ thống hiển thị bảng điểm trung bình theo từng môn của tất cả học sinh trong lớp

*Xem bảng điểm chi tiết của từng học sinh:*
1. Giáo viên click vào nút bảng điểm của học sinh
2. Hệ thống hiển thị bảng điểm chi tiết (tất cả cột điểm, hệ số, ĐTB) của học sinh đó

**Input Data:**
- Không có input (hệ thống hiển thị tự động)

**Output Data:**
- Bảng điểm trung bình các môn của tất cả học sinh trong lớp (kèm cột Tổng ĐTB)
- Bảng điểm chi tiết của từng học sinh (khi click xem chi tiết)

**Validation Rules:**
- Không áp dụng

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-011-01 | GVCN chỉ xem bảng điểm, không chỉnh sửa điểm |
| BR-011-02 | Bảng điểm hiển thị ĐTB tất cả các môn học của lớp |
| BR-011-03 | Có nút xem bảng điểm chi tiết cho từng học sinh |
| BR-011-04 | Bảng điểm tổng hợp có thêm cột "Tổng ĐTB" = trung bình cộng của ĐTB tất cả các môn học (chỉ tính các môn đã có ĐTB) |
| BR-011-05 | Màu sắc hiển thị ĐTB: ≥ 8 → xanh lá, 5 ≤ ĐTB < 8 → cam, < 5 → đỏ (áp dụng cho cả ĐTB từng môn và Tổng ĐTB) |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-011-01 | Chưa có điểm (GVBM chưa nhập) | Hiển thị trống / "Chưa có điểm" |

---

### FR-012: Lớp chủ nhiệm – Xem thông tin học sinh

**Mô tả chức năng:**
Giáo viên chủ nhiệm xem chi tiết thông tin của từng học sinh trong lớp.

**Actor:** Giáo viên (vai trò GVCN)

**Business Flow:**
1. Giáo viên chọn học sinh trong lớp chủ nhiệm
2. Hệ thống hiển thị chi tiết thông tin học sinh

**Input Data:**
- Không có input (chọn học sinh để xem)

**Output Data:**
- Thông tin chi tiết học sinh (theo cấu trúc FR-006)

**Validation Rules:**
- Không áp dụng

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-012-01 | GVCN chỉ xem được thông tin học sinh trong lớp mình chủ nhiệm |

**Exception Handling:**
- Không có exception đặc biệt

---

### FR-013: Quản lý nhận xét học sinh

**Mô tả chức năng:**
Giáo viên chủ nhiệm tạo nhận xét cho học sinh trong lớp với thời gian gửi lên lịch trong tương lai. Khi đến thời điểm đã lên lịch, hệ thống tự động gửi thông báo đến phụ huynh và học sinh.

**Actor:** Giáo viên (vai trò GVCN)

**Business Flow:**

*Tạo nhận xét:*
1. Giáo viên chọn học sinh trong lớp chủ nhiệm
2. Chọn chức năng tạo nhận xét
3. Nhập nội dung nhận xét và thời gian gửi (phải là thời gian trong tương lai)
4. Hệ thống lưu nhận xét với trạng thái "Đã lên lịch"
5. Hiển thị thông báo thành công

*Chỉnh sửa nhận xét:*
1. Giáo viên chọn nhận xét có trạng thái "Đã lên lịch"
2. Chỉnh sửa nội dung hoặc thời gian gửi
3. Hệ thống lưu cập nhật

*Gửi tự động (System job):*
1. Hệ thống kiểm tra các nhận xét "Đã lên lịch" có thời gian gửi đã đến
2. Gửi email thông báo đến email đã đăng ký của phụ huynh và học sinh
3. Tiêu đề email: "Nhận xét mới từ giáo viên"
4. Nội dung email: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm."
5. Chuyển trạng thái nhận xét thành "Đã gửi"

*Xem lịch sử nhận xét:*
1. Giáo viên truy cập danh sách nhận xét
2. Hệ thống hiển thị danh sách sắp xếp theo thời gian mới nhất

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Học sinh | Reference | Có | Học sinh được nhận xét |
| Thời gian gửi | DateTime | Có | Thời gian gửi (phải ở tương lai) |
| Nội dung nhận xét | Text | Có | Nội dung nhận xét |

**Output Data:**
- Danh sách nhận xét (sắp xếp mới nhất)
- Chi tiết nhận xét (nội dung, thời gian, trạng thái)
- Thông báo tự động đến phụ huynh và học sinh

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-013-01 | Nội dung nhận xét không được để trống | Bắt buộc |
| VR-013-02 | Thời gian gửi không được để trống | Bắt buộc |
| VR-013-03 | Thời gian gửi phải ở tương lai | Logic thời gian |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-013-01 | "Vui lòng nhập nội dung nhận xét" | Nội dung để trống |
| MSG-013-02 | "Vui lòng chọn thời gian gửi" | Thời gian để trống |
| MSG-013-03 | "Thời gian gửi phải là thời gian trong tương lai" | Thời gian <= hiện tại |
| MSG-013-06 | "Năm học đã kết thúc, không thể thêm hoặc chỉnh sửa nhận xét" | Thao tác trên nhận xét khi năm học đã kết thúc |
| MSG-013-04 | Tiêu đề email: "Nhận xét mới từ giáo viên" | Email thông báo gửi đến phụ huynh/học sinh |
| MSG-013-05 | Nội dung email: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm." | Nội dung email thông báo |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-013-01 | Khi tạo nhận xét → trạng thái mặc định là "Đã lên lịch" |
| BR-013-02 | Chỉ được chỉnh sửa nhận xét khi trạng thái còn "Đã lên lịch" |
| BR-013-03 | Khi đến thời điểm gửi: hệ thống tự động gửi email thông báo đến email đã đăng ký của phụ huynh và học sinh |
| BR-013-04 | Tiêu đề email: "Nhận xét mới từ giáo viên" |
| BR-013-08 | Nội dung email: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm." |
| BR-013-05 | Sau khi gửi, trạng thái chuyển thành "Đã gửi" |
| BR-013-06 | Nhận xét đã gửi không thể chỉnh sửa (suy luận từ BR-013-02) |
| BR-013-07 | Lịch sử nhận xét sắp xếp theo thứ tự mới nhất lên trước |
| BR-013-09 | Khi năm học ở trạng thái "Đã kết thúc": không được tạo mới hoặc chỉnh sửa nhận xét thuộc năm học đó (BR-003-03) |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-013-01 | Chỉnh sửa nhận xét đã gửi | Không cho phép (BR-013-06) |
| EX-013-02 | Thời gian gửi đã qua khi lưu | Hiển thị MSG-013-03 |
| EX-013-03 | Gửi thông báo thất bại (lỗi hệ thống) | BRD không đề cập |
| EX-013-04 | Xóa nhận xét | BRD không đề cập |
| EX-013-05 | Thêm mới hoặc chỉnh sửa nhận xét khi năm học đã kết thúc | Hiển thị MSG-013-06, không thực hiện thao tác |

---

### FR-014: Lớp bộ môn – Quản lý điểm

**Mô tả chức năng:**
Tại dashboard, hiển thị danh sách lớp mà giáo viên phụ trách bộ môn kèm thông tin lớp và bộ môn. Khi click vào xem chi tiết, hiển thị bảng điểm chi tiết. Giáo viên chỉnh sửa điểm trực tiếp tại bảng, nhấn "Lưu" để lưu và tính lại điểm.

**Actor:** Giáo viên (vai trò GVBM)

**Business Flow:**
1. Giáo viên chọn lớp bộ môn từ dashboard (hiển thị lớp + bộ môn phụ trách)
2. Click vào xem chi tiết
3. Hệ thống hiển thị bảng điểm chi tiết của học sinh trong bộ môn đó
4. Giáo viên chỉnh sửa điểm trực tiếp tại bảng
5. Nhấn nút "Lưu" → hệ thống lưu điểm và tính toán lại ĐTB
6. Khi cần thêm cột điểm mới: click nút "Thêm điểm", tại title cột chọn hệ số để nhập điểm

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Cột điểm | Object | Có | Cột điểm mới (kèm hệ số và tên cột tuỳ chọn) |
| Tên cột | String | Không | Tên cột điểm tuỳ chọn, tối đa 20 ký tự (VD: "Kiểm tra 15 phút", "Giữa kỳ") |
| Hệ số điểm | Number | Có | Hệ số của cột điểm (1, 2 hoặc 3) |
| Điểm | Number | Có | Giá trị điểm cho từng học sinh (0–10, hỗ trợ nhập dấu phẩy thập phân) |

**Output Data:**
- Bảng điểm chi tiết (các cột điểm + hệ số + ĐTB)
- Điểm trung bình tự động tính

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-014-01 | Phải chọn hệ số khi thêm cột điểm | Bắt buộc |
| VR-014-02 | Điểm phải là số hợp lệ trong khoảng 0–10 | Thang điểm 10 |
| VR-014-03 | Tên cột không được vượt quá 20 ký tự | Tùy chọn nhưng có giới hạn độ dài |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-014-01 | "Vui lòng chọn hệ số điểm" | Chưa chọn hệ số |
| MSG-014-02 | "Điểm không hợp lệ" | Điểm nhập sai format |
| MSG-014-03 | "Năm học đã kết thúc, không thể chỉnh sửa điểm" | Thao tác sửa điểm khi năm học đã kết thúc |
| MSG-014-04 | "Tên cột không được vượt quá 20 ký tự" | Tên cột vượt giới hạn ký tự |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-014-01 | GVBM chỉ quản lý điểm của lớp và môn mình được phân công |
| BR-014-02 | Mỗi cột điểm có hệ số riêng |
| BR-014-03 | Chỉnh sửa điểm trực tiếp trên bảng (inline editing), sau đó nhấn nút "Lưu" |
| BR-014-04 | Sau khi nhấn "Lưu", hệ thống lưu điểm và tính toán lại ĐTB |
| BR-014-05 | Thêm cột điểm mới qua nút "Thêm cột điểm"; form thêm có trường tên cột (tuỳ chọn, tối đa 20 ký tự) và hệ số (bắt buộc). Header cột hiển thị tên cột nếu có, ngược lại hiển thị "Cột {số thứ tự}" |
| BR-014-06 | Khi năm học ở trạng thái "Đã kết thúc": không được thêm cột điểm mới hoặc chỉnh sửa điểm thuộc năm học đó (BR-003-03) |
| BR-014-07 | Điểm hỗ trợ nhập số thập phân bằng dấu phẩy (,) hoặc dấu chấm (.), hệ thống tự chuyển về định dạng số thập phân (VD: "7,5" → 7.5) |
| BR-014-08 | Màu sắc hiển thị ĐTB tại cột ĐTB: ≥ 8 → xanh lá, 5 ≤ ĐTB < 8 → cam, < 5 → đỏ |

> **Lưu ý:** BRD không đề cập số lượng cột điểm tối đa.

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-014-01 | Xóa cột điểm đã có dữ liệu | BRD không đề cập |
| EX-014-02 | Nhập điểm ngoài thang | BRD không đề cập thang điểm |
| EX-014-03 | Thêm cột điểm hoặc chỉnh sửa điểm khi năm học đã kết thúc | Hiển thị MSG-014-03, không thực hiện thao tác |

---

### FR-015: Công thức tính điểm trung bình

**Mô tả chức năng:**
Hệ thống tự động tính điểm trung bình môn học cho từng học sinh dựa trên điểm các cột và hệ số tương ứng. Công thức sử dụng trung bình có trọng số (weighted average).

**Actor:** Hệ thống (tự động)

**Business Flow:**
1. GVBM nhập hoặc cập nhật điểm, nhấn "Lưu"
2. Hệ thống tính lại ĐTB theo công thức có trọng số hệ số

**Input Data:**
- Tất cả điểm thành phần và hệ số tương ứng của học sinh

**Output Data:**
- Điểm trung bình môn học

**Validation Rules:**
- Không áp dụng (hệ thống tự tính)

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-015-01 | ĐTB = Tổng (Điểm × Hệ số) / Tổng Hệ số |
| BR-015-02 | ĐTB được tính lại khi GVBM nhấn "Lưu" |

**Ví dụ minh họa:**

Giả sử học sinh có:
- 3 con điểm hệ số 1: lần lượt là 7, 8, 9
- 2 con điểm hệ số 2: lần lượt là 7, 9
- 1 con điểm hệ số 3: là 8

```
ĐTB = (7 + 8 + 9 + 2×(7 + 9) + 3×8) / (1 + 1 + 1 + 2 + 2 + 3)
     = (7 + 8 + 9 + 14 + 18 + 24) / 10
     = 80 / 10
     = 8.0
```

Trong đó `(1 + 1 + 1 + 2 + 2 + 3) = 10` là tổng hệ số.

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-015-01 | Chưa có điểm nào để tính ĐTB | Hiển thị trống / không tính |
| EX-015-02 | Chỉ có một phần cột điểm có giá trị | BRD không đề cập (tính trên dữ liệu có sẵn hay chờ đủ?) |

---

### FR-016: Cổng thông tin phụ huynh

**Mô tả chức năng:**
Phụ huynh đăng nhập hệ thống để xem điểm và nhận xét của con mình. Nếu phụ huynh có nhiều con trong hệ thống, phụ huynh chọn từng con từ dashboard để xem thông tin.

**Actor:** Phụ huynh

**Business Flow:**

*Phụ huynh có 1 con:*
1. Phụ huynh đăng nhập hệ thống
2. Hệ thống tự động hiển thị bảng điểm và nhận xét của con

*Phụ huynh có nhiều con:*
1. Phụ huynh đăng nhập hệ thống
2. Hệ thống hiển thị danh sách các con đang liên kết với tài khoản phụ huynh
3. Phụ huynh chọn 1 con từ danh sách
4. Hệ thống hiển thị bảng điểm và nhận xét của con được chọn
5. Phụ huynh có thể quay lại và chọn con khác

**Input Data:**

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Học sinh được chọn | Reference | Không (chỉ khi có nhiều con) | Chọn học sinh từ danh sách các con |

**Output Data:**
- Danh sách các con liên kết với tài khoản (hiển thị khi có nhiều con)
- Bảng điểm của con được chọn (theo môn, theo lớp)
- Danh sách nhận xét từ giáo viên của con được chọn
- Email thông báo nhận xét mới

**Validation Rules:**
- Không áp dụng

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-016-01 | Phụ huynh chỉ xem được thông tin của các con liên kết với tài khoản mình |
| BR-016-02 | Phụ huynh nhận email thông báo khi nhận xét được gửi. Tiêu đề: "Nhận xét mới từ giáo viên". Nội dung: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm." |
| BR-016-03 | Phụ huynh chỉ có quyền xem, không có quyền chỉnh sửa |
| BR-016-04 | Nếu phụ huynh có 1 con: tự động hiển thị thông tin con đó sau khi đăng nhập |
| BR-016-05 | Nếu phụ huynh có nhiều con: hiển thị danh sách các con để chọn trước khi xem chi tiết |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-016-01 | Con chưa được xếp lớp | BRD không đề cập |
| EX-016-02 | Chưa có điểm hoặc nhận xét | Hiển thị trống |

---

### FR-017: Cổng thông tin học sinh

**Mô tả chức năng:**
Học sinh đăng nhập hệ thống để xem điểm và nhận xét cá nhân.

**Actor:** Học sinh

**Business Flow:**
1. Học sinh đăng nhập hệ thống
2. Hệ thống hiển thị điểm và nhận xét cá nhân
3. Học sinh nhận thông báo qua email đã đăng ký khi có nhận xét mới từ giáo viên

**Input Data:**
- Không có input (chỉ xem)

**Output Data:**
- Bảng điểm cá nhân (theo môn)
- Danh sách nhận xét từ giáo viên
- Email thông báo nhận xét mới

**Validation Rules:**
- Không áp dụng

**Message Validation:**
- Không áp dụng

**Business Rules:**

| STT | Rule |
|---|---|
| BR-017-01 | Học sinh chỉ xem được thông tin điểm và nhận xét của chính mình |
| BR-017-02 | Học sinh nhận email thông báo khi nhận xét được gửi. Tiêu đề: "Nhận xét mới từ giáo viên". Nội dung: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm." |
| BR-017-03 | Học sinh chỉ có quyền xem, không có quyền chỉnh sửa |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-017-01 | Chưa được xếp lớp | BRD không đề cập |
| EX-017-02 | Chưa có điểm hoặc nhận xét | Hiển thị trống |

---

### FR-018: Quên mật khẩu

**Mô tả chức năng:**
Tại màn hình đăng nhập, người dùng có thể yêu cầu đặt lại mật khẩu thông qua link "Quên mật khẩu". Hệ thống gửi email chứa link đặt lại mật khẩu. Link có hiệu lực trong 15 phút.

**Actor:** Admin System, Admin School, Giáo viên, Phụ huynh, Học sinh

**Business Flow:**

*Yêu cầu đặt lại mật khẩu:*
1. Người dùng nhấn link "Quên mật khẩu" tại màn hình đăng nhập
2. Hệ thống hiển thị form nhập email
3. Người dùng nhập email đã đăng ký
4. Hệ thống kiểm tra email tồn tại trong hệ thống
5. Nếu email tồn tại → hệ thống tạo token đặt lại mật khẩu và gửi email chứa link đặt lại
6. Hiển thị thông báo "Vui lòng kiểm tra email để đặt lại mật khẩu"

*Đặt lại mật khẩu:*
1. Người dùng nhấn vào link trong email
2. Hệ thống kiểm tra token còn hiệu lực (trong 15 phút)
3. Nếu token hợp lệ → hiển thị form nhập mật khẩu mới
4. Người dùng nhập mật khẩu mới và xác nhận mật khẩu mới
5. Hệ thống cập nhật mật khẩu và chuyển về màn hình đăng nhập
6. Hiển thị thông báo "Đặt lại mật khẩu thành công"

**Input Data:**

*Form yêu cầu đặt lại:*

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Email | String (Email) | Có | Email đã đăng ký trong hệ thống |

*Form đặt lại mật khẩu:*

| Trường | Kiểu dữ liệu | Bắt buộc | Mô tả |
|---|---|---|---|
| Mật khẩu mới | String | Có | Mật khẩu mới |
| Xác nhận mật khẩu mới | String | Có | Nhập lại mật khẩu mới để xác nhận |

**Output Data:**
- Email chứa link đặt lại mật khẩu
- Thông báo kết quả (thành công / thất bại)

**Validation Rules:**

| STT | Rule | Mô tả |
|---|---|---|
| VR-018-01 | Email không được để trống | Bắt buộc |
| VR-018-02 | Email phải đúng định dạng | Format email |
| VR-018-03 | Mật khẩu mới không được để trống | Bắt buộc |
| VR-018-04 | Xác nhận mật khẩu mới không được để trống | Bắt buộc |
| VR-018-05 | Mật khẩu mới và xác nhận mật khẩu phải trùng khớp | Logic kiểm tra |

**Message Validation:**

| Code | Message | Điều kiện |
|---|---|---|
| MSG-018-01 | "Vui lòng nhập email" | Email để trống |
| MSG-018-02 | "Email không đúng định dạng" | Email sai format |
| MSG-018-03 | "Vui lòng kiểm tra email để đặt lại mật khẩu" | Email tồn tại trong hệ thống, đã gửi email |
| MSG-018-04 | "Email không tồn tại trong hệ thống" | Email không tìm thấy |
| MSG-018-05 | "Vui lòng nhập mật khẩu mới" | Mật khẩu mới để trống |
| MSG-018-06 | "Vui lòng xác nhận mật khẩu mới" | Xác nhận mật khẩu để trống |
| MSG-018-07 | "Mật khẩu mới và xác nhận mật khẩu không trùng khớp" | 2 trường mật khẩu khác nhau |
| MSG-018-08 | "Đặt lại mật khẩu thành công" | Cập nhật mật khẩu thành công |
| MSG-018-09 | "Link đặt lại mật khẩu đã hết hạn" | Token hết hiệu lực (quá 15 phút) |
| MSG-018-10 | Tiêu đề email: "Đặt lại mật khẩu" | Tiêu đề email gửi đến người dùng |
| MSG-018-11 | Nội dung email: "Bạn vừa yêu cầu đặt lại mật khẩu. Vui lòng nhấn vào link bên dưới để đặt lại mật khẩu. Link có hiệu lực trong 15 phút." | Nội dung email gửi đến người dùng |

**Business Rules:**

| STT | Rule |
|---|---|
| BR-018-01 | Link đặt lại mật khẩu có hiệu lực trong 15 phút kể từ khi gửi |
| BR-018-02 | Sau khi đặt lại mật khẩu thành công, token bị vô hiệu hóa (không dùng lại được) |
| BR-018-03 | Tiêu đề email: "Đặt lại mật khẩu" |
| BR-018-04 | Nội dung email: "Bạn vừa yêu cầu đặt lại mật khẩu. Vui lòng nhấn vào link bên dưới để đặt lại mật khẩu. Link có hiệu lực trong 15 phút." |
| BR-018-05 | Sau khi đặt lại thành công, chuyển về màn hình đăng nhập |
| BR-018-06 | Chức năng áp dụng cho tất cả role trong hệ thống |

**Exception Handling:**

| STT | Exception | Xử lý |
|---|---|---|
| EX-018-01 | Email không tồn tại trong hệ thống | Hiển thị MSG-018-04 |
| EX-018-02 | Link đặt lại mật khẩu đã hết hạn (quá 15 phút) | Hiển thị MSG-018-09 |
| EX-018-03 | Token đã được sử dụng | Hiển thị MSG-018-09 |
| EX-018-04 | Mật khẩu mới và xác nhận không trùng khớp | Hiển thị MSG-018-07 |
| EX-018-05 | Gửi email thất bại (lỗi hệ thống) | Hiển thị thông báo lỗi hệ thống |

---

## 5. Non-functional Requirements

| STT | Loại | Yêu cầu | Ghi chú |
|---|---|---|---|
| NFR-001 | Bảo mật | Hệ thống phân quyền theo role, mỗi role chỉ truy cập được chức năng và dữ liệu trong phạm vi cho phép | Từ BRD |
| NFR-002 | Bảo mật | Mật khẩu phải được mã hóa khi lưu trữ | BRD không đề cập – khuyến nghị bổ sung |
| NFR-003 | Hiệu năng | Hệ thống hỗ trợ nhiều trường học đồng thời (multi-tenant) | Suy luận từ kiến trúc Admin System → nhiều trường |
| NFR-004 | Tự động hóa | Hệ thống có cơ chế job tự động gửi nhận xét đúng thời gian đã lên lịch | Từ BRD mục 4.3.3 |
| NFR-005 | Thông báo | Hệ thống hỗ trợ gửi thông báo qua email đến phụ huynh và học sinh. Tiêu đề: "Nhận xét mới từ giáo viên". Nội dung: "Giáo viên vừa có nhận xét mới về học sinh, hãy truy cập hệ thống để xem chi tiết. Xin chân thành cảm ơn phụ huynh và học sinh đã quan tâm." | Từ BRD mục 4.3.3 |
| NFR-006 | Giao diện | Hỗ trợ chỉnh sửa điểm trực tiếp trên bảng (inline editing) | Từ BRD mục 4.3.4 |
| NFR-007 | Giao diện | Hỗ trợ popup chọn học sinh khi thêm vào lớp | Từ BRD mục 4.2.6 |
| NFR-008 | Công nghệ – FE | Giao diện người dùng sử dụng mô hình Client-side Rendering (CSR) | Yêu cầu kỹ thuật dự án |
| NFR-009 | Công nghệ – BE | Backend cần hỗ trợ: REST API, phân quyền theo role, cron job tự động, gửi email | Suy luận từ BRD 4.3.3, 3.2, 2.3 |
| NFR-010 | Công nghệ – DB | Cơ sở dữ liệu phải là hệ quản trị quan hệ (RDBMS), hỗ trợ FK, CHECK constraint, partial unique index | Suy luận từ ERD (15 entity, quan hệ phức tạp) |

> **Lưu ý:** BRD không đề cập các yêu cầu phi chức năng về: hiệu năng cụ thể (response time, concurrent users), khả năng mở rộng, backup/recovery, audit log, trình duyệt hỗ trợ, responsive design, ngôn ngữ hệ thống, kênh gửi thông báo (email, in-app, SMS).

---

## 6. Assumptions & Constraints

### 6.1 Assumptions (Giả định)

| STT | Giả định |
|---|---|
| AS-001 | Mỗi trường học hoạt động độc lập, dữ liệu không chia sẻ giữa các trường |
| AS-002 | Admin School chỉ quản lý dữ liệu trong phạm vi trường mình được gán |
| AS-003 | Giáo viên có thể vừa là GVCN vừa là GVBM (khác lớp hoặc cùng lớp) |
| AS-004 | Thông báo nhận xét được gửi qua email đã đăng ký của phụ huynh và học sinh |
| AS-005 | Hệ thống hoạt động theo niên khóa (năm học), dữ liệu được tổ chức theo năm học |
| AS-006 | Trạng thái mặc định khi tạo năm học là "Chuẩn bị diễn ra" |

### 6.2 Constraints (Ràng buộc)

| STT | Ràng buộc |
|---|---|
| CO-001 | Tại một thời điểm, chỉ có tối đa 1 năm học ở trạng thái "Đang diễn ra" |
| CO-002 | 1 giáo viên chỉ chủ nhiệm tối đa 1 lớp trong cùng năm học |
| CO-003 | 1 học sinh chỉ thuộc 1 lớp trong cùng năm học |
| CO-004 | Tên lớp không được trùng trong cùng năm học |
| CO-005 | CCCD giáo viên phải duy nhất |
| CO-006 | Mã số giáo viên phải duy nhất |

### 6.3 Các điểm cần làm rõ với Stakeholder

| STT | Câu hỏi | Trạng thái |
|---|---|---|
| Q-001 | Tài khoản Admin School được tạo tự động: thông tin đăng nhập mặc định (email, password) là gì? | Chưa rõ |
| Q-002 | Cơ chế chuyển trạng thái năm học: tự động theo thời gian hay thủ công bởi Admin School? | Chưa rõ |
| ~~Q-003~~ | ~~Các giá trị cụ thể của trạng thái làm việc giáo viên?~~ | **Đã giải đáp:** Đang làm việc / Đã nghỉ |
| ~~Q-004~~ | ~~Các giá trị cụ thể của trạng thái học sinh?~~ | **Đã giải đáp:** Đang học / Đã nghỉ |
| Q-005 | Thang điểm: 0-10 hay 0-100? | Chưa rõ |
| Q-006 | Các giá trị hệ số điểm cụ thể (1, 2, 3...)? | Chưa rõ |
| ~~Q-007~~ | ~~Công thức tính ĐTB chính xác?~~ | **Đã giải đáp:** ĐTB = Tổng(Điểm × Hệ số) / Tổng Hệ số |
| ~~Q-008~~ | ~~Kênh gửi thông báo nhận xét: in-app, email, SMS?~~ | **Đã giải đáp:** Gửi qua email đã đăng ký của phụ huynh và học sinh |
| Q-009 | Có chức năng xóa cho giáo viên, học sinh, lớp, điểm không? | Chưa rõ |
| ~~Q-010~~ | ~~Chi tiết các trường dữ liệu của trường học (entity School)?~~ | **Đã giải đáp:** Tên trường, Mã trường (unique, tự nhập) |
| ~~Q-011~~ | ~~Dòng "indicate." ở mục 4.3.2 có ý nghĩa gì?~~ | **Đã giải đáp:** Xem bảng điểm chi tiết của từng học sinh |

---

## 7. Technology Stack

> Phần này ghi nhận các công nghệ được khuyến nghị sử dụng cho dự án, dựa trên phân tích yêu cầu từ BRD v1.1, SRS v1.1 và ERD v1.1.

---

### 7.1 Database: PostgreSQL

**Lý do lựa chọn:**

| Yêu cầu hệ thống | Hỗ trợ từ PostgreSQL |
|---|---|
| 15 entity với quan hệ FK phức tạp | RDBMS với đầy đủ hỗ trợ FK, JOIN nhiều bảng |
| CHECK constraint (`school_code`, `status`, `coefficient > 0`) | Native CHECK constraint |
| Chỉ 1 năm học `in_progress` / trường (BR-003-01) | Partial Unique Index |
| ENUM (`role`, `status`, `work_status`, `study_status`) | Native ENUM type |
| Tìm kiếm tên trường, mã trường (FR-002) | Full-text search, Index support |
| UUID làm khóa chính | Native UUID type |
| Tính ĐTB có trọng số (complex JOIN + aggregate) | Hiệu năng cao với complex query |

---

### 7.2 Backend: NestJS (Node.js + TypeScript)

**Framework:** NestJS

**Lý do lựa chọn:**

| Yêu cầu hệ thống | Thư viện / Cơ chế |
|---|---|
| Phân quyền 5 role (FR-001, BR-001-01) | `@nestjs/passport` + JWT + Custom `RoleGuard` |
| Cron job gửi email nhận xét theo lịch (FR-013, BR-013-03) | `@nestjs/schedule` — quét `comments` có `scheduled_at <= NOW()` |
| Gửi email (thông báo nhận xét + reset password) | `@nestjs-modules/mailer` + Nodemailer |
| ORM kết nối PostgreSQL | **Prisma** (type-safe, schema-first, dễ migrate) |
| Validation (VR-xxx trong SRS) | `class-validator` + `class-transformer` |
| Cấu trúc module theo nghiệp vụ | NestJS Modules (chia theo role / chức năng) |

**Cấu trúc module gợi ý:**

```
src/
  auth/             → Đăng nhập, quên mật khẩu (FR-001, FR-018)
  schools/          → Quản lý trường học (FR-002)
  academic-years/   → Quản lý năm học (FR-003)
  subjects/         → Quản lý môn học (FR-004)
  teachers/         → Quản lý giáo viên (FR-005)
  students/         → Quản lý học sinh + phụ huynh (FR-006)
  classes/          → Quản lý lớp học + phân công (FR-007, FR-008, FR-009)
  scores/           → Quản lý điểm số, tính ĐTB (FR-014, FR-015)
  comments/         → Quản lý nhận xét + cron job gửi email (FR-013)
  notifications/    → Lưu log email đã gửi (FR-013)
  mail/             → Module gửi email dùng chung
```

**Ghi chú email production:** Khuyến nghị dùng **SendGrid** hoặc **AWS SES** thay vì SMTP thuần để đảm bảo email không vào spam và có tracking trạng thái gửi (cập nhật `notifications.is_sent`, `sent_at`).

---

### 7.3 Frontend: React + Vite (CSR)

**Rendering:** Client-side Rendering (CSR) — theo NFR-008

**Lý do lựa chọn:**

| Thư viện | Mục đích |
|---|---|
| **React + Vite** | Framework CSR, build nhanh, hot reload |
| **Ant Design (antd)** | UI component library — phù hợp hệ thống admin nhiều bảng/form phức tạp |
| **TanStack Query** | Quản lý server state: fetch API, caching, auto-refetch |
| **Zustand** | Quản lý client state: thông tin auth, role, con đang chọn (parent portal) |
| **React Router v6** | Client-side routing theo role |

**Ant Design phù hợp vì:**

| Component antd | Dùng tại màn hình |
|---|---|
| `Table` với sort / filter / pagination | Bảng điểm, danh sách HS, GV, trường (FR-002, FR-005, FR-006, FR-015) |
| Inline editable `Table` | Nhập điểm trực tiếp trên bảng (FR-014) |
| `Form` + validation | CRUD tất cả entity |
| `Modal` / `Drawer` | Popup thêm HS vào lớp (FR-008), cảnh báo xóa trường (FR-002 EX-002-05) |
| `DatePicker`, `TimePicker` | Lên lịch gửi nhận xét (FR-013) |
| `Select` | Chọn GVCN, GVBM, môn học, năm học |
| `Select` / `Tabs` chọn con | Parent portal — chọn con để xem điểm (FR-016) |

---

### 7.4 Tổng hợp stack

```
┌──────────────────────────────────────────────────┐
│  Frontend (CSR)                                  │
│  React + Vite + Ant Design                       │
│  TanStack Query | Zustand | React Router v6      │
├──────────────────────────────────────────────────┤
│  Backend                                         │
│  NestJS (Node.js + TypeScript)                   │
│  Prisma ORM | JWT Auth | @nestjs/schedule        │
│  Nodemailer / SendGrid (Email)                   │
├──────────────────────────────────────────────────┤
│  Database                                        │
│  PostgreSQL                                      │
└──────────────────────────────────────────────────┘
```

---

*Kết thúc tài liệu SRS v1.1*
