# TÀI LIỆU BRD – HỆ THỐNG QUẢN LÝ ĐIỂM HỌC SINH

---

## 1. Thông tin tài liệu

| Mục | Nội dung |
|---|---|
| Tên dự án | Hệ thống Quản lý điểm học sinh |
| Người soạn | Business Analyst |
| Ngày tạo | 12/02/2026 |
| Phiên bản | 1.0 |

---

## 2. Tổng quan hệ thống

### 2.1 Mục tiêu hệ thống

Hệ thống được xây dựng nhằm:

- Quản lý dữ liệu trường học
- Quản lý thông tin giáo viên, học sinh, lớp học
- Quản lý điểm số và nhận xét học sinh
- Hỗ trợ nhiều vai trò người dùng
- Tự động gửi nhận xét đến phụ huynh và học sinh theo lịch

---

### 2.2 Phạm vi hệ thống

Hệ thống bao gồm:

- Quản lý người dùng theo vai trò
- Quản lý master data giáo dục
- Quản lý lớp học và phân công giảng dạy
- Quản lý điểm và tính điểm trung bình
- Quản lý nhận xét học sinh
- Cổng thông tin phụ huynh và học sinh

---

### 2.3 Vai trò người dùng

| Vai trò | Mô tả |
|---|---|
| Admin System | Quản lý danh sách trường học |
| Admin School | Quản lý dữ liệu trường |
| Giáo viên | Quản lý điểm và nhận xét |
| Phụ huynh | Xem điểm và nhận xét của con |
| Học sinh | Xem điểm và nhận xét cá nhân |

---

## 3. Mô tả nghiệp vụ tổng thể

### 3.1 Đăng nhập
- Hệ thống cho phép đăng nhập theo nhiều role
- User được phân quyền theo role

---

## 4. Yêu cầu nghiệp vụ chi tiết

---

# 4.1 Admin System

## 4.1.1 Quản lý trường học

### Chức năng
- CRUD trường học

### Quy tắc nghiệp vụ
- Khi tạo trường học, hệ thống tự động tạo tài khoản Admin School

### Thông tin tài khoản mặc định
- Email: adminstpoint@gmail.com
- Password: adminstpoiNt1122@

---

# 4.2 Admin School

---

## 4.2.1 Quản lý năm học

### Thông tin
- Tên năm học
- Thời gian bắt đầu
- Thời gian kết thúc
- Trạng thái:
  - Chuẩn bị diễn ra
  - Đang diễn ra
  - Đã kết thúc

### Quy tắc
- Chỉ có 1 năm học đang diễn ra tại một thời điểm

---

## 4.2.2 Quản lý môn học
- Mỗi năm học có danh sách môn học riêng

---

## 4.2.3 Quản lý giáo viên

### Thông tin
- Họ tên
- Ngày sinh
- CCCD (Unique)
- Mã số giáo viên (Unique)
- Giới tính
- Trạng thái làm việc
- Email
- Mật khẩu

---

## 4.2.4 Quản lý học sinh

### Thông tin học sinh
- Họ tên
- Ngày sinh
- Địa chỉ
- Giới tính
- Email đăng nhập
- Mật khẩu
- ID
- Trạng thái học

### Thông tin phụ huynh
- Họ tên cha
- Nghề nghiệp cha
- Ngày sinh cha
- Họ tên mẹ
- Nghề nghiệp mẹ
- Email đăng nhập
- Số điện thoại
- Mật khẩu

---

## 4.2.5 Quản lý lớp học

### Thông tin
- Tên lớp
- Năm học
- Giáo viên chủ nhiệm

### Quy tắc
- Không trùng tên lớp trong cùng năm học
- 1 giáo viên chỉ chủ nhiệm 1 lớp trong năm học
- Giáo viên đã là GVCN sẽ không hiển thị trong danh sách chọn lớp khác

---

## 4.2.6 Quản lý danh sách học sinh trong lớp

### Chức năng
- Thêm học sinh qua popup
- Chỉ hiển thị học sinh trạng thái "Đang học"

### Quy tắc
- 1 học sinh chỉ thuộc 1 lớp trong cùng năm học

---

## 4.2.7 Phân công giáo viên bộ môn

### Chức năng
- Thêm môn học
- Chọn giáo viên phụ trách

---

# 4.3 Giáo viên

---

## 4.3.1 Dashboard

Hiển thị:

- Lớp chủ nhiệm
- Lớp bộ môn

---

## 4.3.2 Lớp chủ nhiệm

### Chức năng
- Xem bảng điểm trung bình
- indicate.
- Xem thông tin học sinh
- Xem nhận xét học sinh

---

## 4.3.3 Quản lý nhận xét học sinh

### Thông tin nhận xét
- Thời gian gửi (tương lai)
- Nội dung nhận xét
- Trạng thái

### Trạng thái
- Đã lên lịch
- Đã gửi

### Quy tắc
1. Khi tạo nhận xét → trạng thái Đã lên lịch
2. Có thể chỉnh sửa trước khi gửi
3. Khi đến thời điểm gửi:
   - Gửi thông báo đến phụ huynh và học sinh
   - Nội dung: "Nhận xét mới từ giáo viên + tên giáo viên"
   - Chuyển trạng thái thành Đã gửi
4. Lịch sử nhận xét sắp xếp mới nhất

---

## 4.3.4 Lớp bộ môn

### Chức năng
- Xem lớp và môn phụ trách
- Chỉnh sửa điểm trực tiếp
- Thêm cột điểm
- Chọn hệ số điểm

---

## 4.3.5 Công thức tính điểm trung bình

