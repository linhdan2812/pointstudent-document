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

## 4.1.1 Tài khoản Admin System

### Thông tin đăng nhập
- Email: adminstpoint@gmail.com
- Password: adminstpoiNt1122@

---

## 4.1.2 Quản lý trường học

### Thông tin
- Tên trường
- Mã trường (Unique, có thể tự nhập)

### Chức năng
- CRUD trường học

### Quy tắc nghiệp vụ
- Mã trường là duy nhất trong hệ thống, Admin System có thể tự nhập
- Khi tạo trường học, hệ thống tự động tạo tài khoản Admin School theo trường học đó

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
- Địa chỉ
- CCCD (Unique)
- Mã số giáo viên (Unique)
- Giới tính
- Trạng thái làm việc:
  - Đang làm việc
  - Đã nghỉ
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
- Trạng thái học:
  - Đang học
  - Đã nghỉ

### Thông tin phụ huynh
- Họ tên cha
- Nghề nghiệp cha
- Ngày sinh cha
- Họ tên mẹ
- Nghề nghiệp mẹ
- Ngày sinh mẹ
- Số điện thoại mẹ
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
- Không trùng tên lớp trong cùng năm học. Nếu trùng tên, hệ thống hiển thị thông báo lỗi và tạo lớp không thành công
- 1 giáo viên chỉ chủ nhiệm 1 lớp trong năm học
- Giáo viên đã là GVCN sẽ không hiển thị trong danh sách chọn lớp khác

---

## 4.2.6 Quản lý danh sách học sinh trong lớp

### Chức năng
- Trong chi tiết lớp học, có mục danh sách học sinh
- Khi nhấn nút "Chỉnh sửa danh sách học sinh", hiển thị popup danh sách học sinh có trạng thái "Đang học"
- Admin School tick checkbox chọn học sinh, sau đó nhấn nút thêm vào lớp

### Quy tắc
- Chỉ hiển thị học sinh có trạng thái "Đang học" trong popup
- 1 học sinh chỉ thuộc 1 lớp trong cùng năm học

---

## 4.2.7 Phân công giáo viên bộ môn

### Chức năng
- Trong chi tiết lớp học, có nút "Thông tin giáo viên bộ môn"
- Khi click, hiển thị trang mới với danh sách gồm 2 cột: Tên môn học và Giáo viên phụ trách
- Tại cột Tên môn học: droplist hiển thị danh sách môn học của năm học đó
- Tại cột Giáo viên: droplist hiển thị danh sách giáo viên để chọn
- Khi cần thêm 1 dòng phân công mới, nhấn nút "Thêm mới"

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
- Xem bảng điểm trung bình theo từng môn của học sinh trong lớp
- Xem bảng điểm chi tiết của từng học sinh
- Xem thông tin cá nhân của học sinh
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
- Tại dashboard, hiển thị danh sách lớp mà giáo viên phụ trách bộ môn kèm thông tin: lớp và bộ môn
- Khi click vào xem chi tiết, hiển thị bảng điểm chi tiết của học sinh trong bộ môn đó
- Chỉnh sửa điểm trực tiếp tại bảng, sau đó nhấn nút "Lưu". Điểm của học sinh sẽ được tính toán lại
- Khi cần thêm 1 cột điểm mới, click nút "Thêm điểm". Tại title cột sẽ chọn được hệ số để nhập điểm

---

## 4.3.5 Công thức tính điểm trung bình

### Công thức

```
ĐTB = Tổng (Điểm × Hệ số) / Tổng Hệ số
```

### Ví dụ minh họa

Giả sử học sinh có:
- 3 con điểm hệ số 1: lần lượt là 7, 8, 9
- 2 con điểm hệ số 2: lần lượt là 7, 9
- 1 con điểm hệ số 3: là 8

**Cách tính:**

```
ĐTB = (7 + 8 + 9 + 2×(7 + 9) + 3×8) / (1 + 1 + 1 + 2 + 2 + 3)

     = (7 + 8 + 9 + 14 + 18 + 24) / 10

     = 80 / 10

     = 8.0
```

Trong đó `(1 + 1 + 1 + 2 + 2 + 3) = 10` là tổng hệ số.

---

# 4.4 Phụ huynh

---

## 4.4.1 Cổng thông tin phụ huynh

### Chức năng
- Phụ huynh sau khi đăng nhập chỉ có thể xem bảng điểm và nhận xét về con của mình
- Nhận thông báo khi có nhận xét mới từ giáo viên

### Quy tắc
- Phụ huynh chỉ xem được thông tin của con mình
- Không có quyền chỉnh sửa bất kỳ thông tin nào

---

# 4.5 Học sinh

---

## 4.5.1 Cổng thông tin học sinh

### Chức năng
- Học sinh sau khi đăng nhập chỉ có thể xem bảng điểm và nhận xét về chính mình
- Nhận thông báo khi có nhận xét mới từ giáo viên

### Quy tắc
- Học sinh chỉ xem được thông tin điểm và nhận xét của chính mình
- Không có quyền chỉnh sửa bất kỳ thông tin nào
