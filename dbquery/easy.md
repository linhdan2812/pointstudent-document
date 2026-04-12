# SQL Practice Exercises — Easy (50 bài)

> Cấp độ: **Easy**
> Đặc điểm: Query 1 bảng, SELECT cơ bản, WHERE, ORDER BY, LIMIT
> Hệ thống: Quản lý Điểm Học sinh

---

### 1.
**Title:** Danh sách tất cả trường học
**Description:** Lấy toàn bộ danh sách trường học trong hệ thống, sắp xếp theo tên trường từ A-Z.
**Tables involved:** `schools`
**Expected Query Goal:** SELECT tất cả cột, ORDER BY name ASC.

---

### 2.
**Title:** Danh sách tài khoản theo vai trò giáo viên
**Description:** Lọc tất cả tài khoản có role = 'teacher' trong bảng users.
**Tables involved:** `users`
**Expected Query Goal:** SELECT * FROM users WHERE role = 'teacher'.

---

### 3.
**Title:** Năm học đang diễn ra
**Description:** Tìm tất cả các năm học có trạng thái 'in_progress'.
**Tables involved:** `academic_years`
**Expected Query Goal:** SELECT * WHERE status = 'in_progress'.

---

### 4.
**Title:** Năm học đã kết thúc
**Description:** Lấy danh sách các năm học có status = 'completed', sắp xếp theo start_date mới nhất trước.
**Tables involved:** `academic_years`
**Expected Query Goal:** SELECT * WHERE status = 'completed' ORDER BY start_date DESC.

---

### 5.
**Title:** Giáo viên đang làm việc
**Description:** Lấy danh sách giáo viên có work_status = 'active', sắp xếp theo full_name.
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE work_status = 'active' ORDER BY full_name ASC.

---

### 6.
**Title:** Giáo viên đã nghỉ việc
**Description:** Lấy danh sách giáo viên có work_status = 'resigned'.
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE work_status = 'resigned'.

---

### 7.
**Title:** Học sinh đang học
**Description:** Lấy toàn bộ học sinh có study_status = 'studying', sắp xếp theo full_name.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * WHERE study_status = 'studying' ORDER BY full_name ASC.

---

### 8.
**Title:** Học sinh đã nghỉ học
**Description:** Tìm tất cả học sinh có study_status = 'dropped_out'.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * WHERE study_status = 'dropped_out'.

---

### 9.
**Title:** Tài khoản phụ huynh
**Description:** Lấy danh sách tất cả tài khoản có role = 'parent'.
**Tables involved:** `users`
**Expected Query Goal:** SELECT * WHERE role = 'parent'.

---

### 10.
**Title:** Tài khoản quản trị hệ thống
**Description:** Tìm tài khoản có role = 'admin_system'.
**Tables involved:** `users`
**Expected Query Goal:** SELECT * WHERE role = 'admin_system'.

---

### 11.
**Title:** Học sinh nữ
**Description:** Lấy danh sách học sinh có gender = 'female', sắp xếp theo full_name.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * WHERE gender = 'female' ORDER BY full_name ASC.

---

### 12.
**Title:** Học sinh nam
**Description:** Lấy danh sách học sinh có gender = 'male'.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * WHERE gender = 'male'.

---

### 13.
**Title:** Nhận xét đã gửi
**Description:** Lấy tất cả nhận xét có status = 'sent', sắp xếp theo created_at mới nhất.
**Tables involved:** `comments`
**Expected Query Goal:** SELECT * WHERE status = 'sent' ORDER BY created_at DESC.

---

### 14.
**Title:** Nhận xét đã lên lịch (chưa gửi)
**Description:** Lấy tất cả nhận xét có status = 'scheduled'.
**Tables involved:** `comments`
**Expected Query Goal:** SELECT * WHERE status = 'scheduled'.

---

### 15.
**Title:** Thông báo chưa gửi email
**Description:** Tìm tất cả notification có is_sent = false.
**Tables involved:** `notifications`
**Expected Query Goal:** SELECT * WHERE is_sent = false.

---

### 16.
**Title:** Thông báo chưa đọc
**Description:** Lấy danh sách notification có is_read = false, sắp xếp theo created_at DESC.
**Tables involved:** `notifications`
**Expected Query Goal:** SELECT * WHERE is_read = false ORDER BY created_at DESC.

---

### 17.
**Title:** Thông báo đã gửi thành công
**Description:** Lấy tất cả notification có is_sent = true kèm thời gian gửi (sent_at).
**Tables involved:** `notifications`
**Expected Query Goal:** SELECT * WHERE is_sent = true ORDER BY sent_at DESC.

---

### 18.
**Title:** Token đặt lại mật khẩu chưa sử dụng
**Description:** Tìm tất cả password_reset_tokens có used_at IS NULL.
**Tables involved:** `password_reset_tokens`
**Expected Query Goal:** SELECT * WHERE used_at IS NULL.

---

### 19.
**Title:** Token đặt lại mật khẩu đã dùng
**Description:** Tìm tất cả password_reset_tokens có used_at IS NOT NULL.
**Tables involved:** `password_reset_tokens`
**Expected Query Goal:** SELECT * WHERE used_at IS NOT NULL.

---

### 20.
**Title:** Token đặt lại mật khẩu đã hết hạn
**Description:** Lấy danh sách token có expires_at < thời điểm hiện tại (đã hết hạn).
**Tables involved:** `password_reset_tokens`
**Expected Query Goal:** SELECT * WHERE expires_at < NOW().

---

### 21.
**Title:** Điểm số chưa nhập (NULL)
**Description:** Lấy tất cả bản ghi điểm có value IS NULL.
**Tables involved:** `scores`
**Expected Query Goal:** SELECT * WHERE value IS NULL.

---

### 22.
**Title:** Điểm số đã nhập
**Description:** Lấy tất cả bản ghi điểm có value IS NOT NULL, sắp xếp theo value DESC.
**Tables involved:** `scores`
**Expected Query Goal:** SELECT * WHERE value IS NOT NULL ORDER BY value DESC.

---

### 23.
**Title:** Điểm số cao (trên 8)
**Description:** Lấy tất cả bản ghi điểm có value > 8.
**Tables involved:** `scores`
**Expected Query Goal:** SELECT * WHERE value > 8.

---

### 24.
**Title:** Điểm số dưới trung bình (dưới 5)
**Description:** Lấy tất cả bản ghi điểm có value < 5 và value IS NOT NULL.
**Tables involved:** `scores`
**Expected Query Goal:** SELECT * WHERE value < 5 AND value IS NOT NULL.

---

### 25.
**Title:** Điểm số trong khoảng 5 đến 8
**Description:** Lấy tất cả bản ghi điểm có value BETWEEN 5 AND 8.
**Tables involved:** `scores`
**Expected Query Goal:** SELECT * WHERE value BETWEEN 5 AND 8.

---

### 26.
**Title:** Cột điểm có hệ số 1
**Description:** Lấy tất cả cột điểm (score_columns) có coefficient = 1.
**Tables involved:** `score_columns`
**Expected Query Goal:** SELECT * WHERE coefficient = 1.

---

### 27.
**Title:** Cột điểm có hệ số 2
**Description:** Lấy tất cả cột điểm có coefficient = 2, sắp xếp theo order ASC.
**Tables involved:** `score_columns`
**Expected Query Goal:** SELECT * WHERE coefficient = 2 ORDER BY `order` ASC.

---

### 28.
**Title:** Cột điểm có hệ số 3
**Description:** Lấy tất cả cột điểm có coefficient = 3.
**Tables involved:** `score_columns`
**Expected Query Goal:** SELECT * WHERE coefficient = 3.

---

### 29.
**Title:** Cột điểm không có nhãn (label NULL)
**Description:** Tìm các cột điểm chưa được đặt tên tùy chỉnh (label IS NULL).
**Tables involved:** `score_columns`
**Expected Query Goal:** SELECT * WHERE label IS NULL.

---

### 30.
**Title:** Cột điểm có nhãn tùy chỉnh
**Description:** Lấy các cột điểm đã được đặt tên (label IS NOT NULL), sắp xếp theo label.
**Tables involved:** `score_columns`
**Expected Query Goal:** SELECT * WHERE label IS NOT NULL ORDER BY label ASC.

---

### 31.
**Title:** Danh sách môn học theo thứ tự tên
**Description:** Lấy tất cả môn học, sắp xếp theo tên từ A-Z.
**Tables involved:** `subjects`
**Expected Query Goal:** SELECT * ORDER BY name ASC.

---

### 32.
**Title:** Tài khoản mới nhất tạo gần đây (top 10)
**Description:** Lấy 10 tài khoản được tạo mới nhất trong hệ thống.
**Tables involved:** `users`
**Expected Query Goal:** SELECT * ORDER BY created_at DESC LIMIT 10.

---

### 33.
**Title:** Học sinh mới nhất (top 5)
**Description:** Lấy 5 học sinh được thêm vào hệ thống gần đây nhất.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * ORDER BY created_at DESC LIMIT 5.

---

### 34.
**Title:** Lớp học sắp xếp theo tên
**Description:** Lấy tất cả lớp học, sắp xếp theo tên lớp từ A-Z.
**Tables involved:** `classes`
**Expected Query Goal:** SELECT * ORDER BY name ASC.

---

### 35.
**Title:** Năm học theo thứ tự bắt đầu
**Description:** Lấy tất cả năm học, sắp xếp theo start_date từ cũ đến mới.
**Tables involved:** `academic_years`
**Expected Query Goal:** SELECT * ORDER BY start_date ASC.

---

### 36.
**Title:** Năm học đang chuẩn bị
**Description:** Lấy danh sách năm học có status = 'preparing'.
**Tables involved:** `academic_years`
**Expected Query Goal:** SELECT * WHERE status = 'preparing'.

---

### 37.
**Title:** Giáo viên nam
**Description:** Lấy danh sách giáo viên có gender = 'male', sắp xếp theo full_name.
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE gender = 'male' ORDER BY full_name ASC.

---

### 38.
**Title:** Giáo viên nữ
**Description:** Lấy danh sách giáo viên có gender = 'female'.
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE gender = 'female'.

---

### 39.
**Title:** Học sinh theo mã học sinh
**Description:** Lấy danh sách học sinh sắp xếp theo student_code từ nhỏ đến lớn.
**Tables involved:** `students`
**Expected Query Goal:** SELECT * ORDER BY student_code ASC.

---

### 40.
**Title:** Tài khoản học sinh
**Description:** Lấy tất cả tài khoản có role = 'student'.
**Tables involved:** `users`
**Expected Query Goal:** SELECT * WHERE role = 'student'.

---

### 41.
**Title:** Nhận xét được tạo trong 7 ngày gần đây
**Description:** Lấy tất cả nhận xét có created_at >= 7 ngày trước (tính từ hôm nay).
**Tables involved:** `comments`
**Expected Query Goal:** SELECT * WHERE created_at >= NOW() - INTERVAL '7 days'.

---

### 42.
**Title:** Nhận xét có lịch gửi trong tương lai
**Description:** Tìm các nhận xét có scheduled_at > NOW() và status = 'scheduled'.
**Tables involved:** `comments`
**Expected Query Goal:** SELECT * WHERE scheduled_at > NOW() AND status = 'scheduled'.

---

### 43.
**Title:** Nhận xét có lịch gửi đã qua nhưng chưa gửi
**Description:** Tìm các nhận xét có scheduled_at < NOW() và status vẫn còn 'scheduled' (có thể bị miss).
**Tables involved:** `comments`
**Expected Query Goal:** SELECT * WHERE scheduled_at < NOW() AND status = 'scheduled'.

---

### 44.
**Title:** Giáo viên có mã CCCD cụ thể
**Description:** Tìm giáo viên theo citizen_id (tra cứu theo CCCD).
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE citizen_id = '...'.

---

### 45.
**Title:** Giáo viên theo mã giáo viên
**Description:** Tìm giáo viên theo teacher_code.
**Tables involved:** `teachers`
**Expected Query Goal:** SELECT * WHERE teacher_code = '...'.

---

### 46.
**Title:** Trường học theo school_code
**Description:** Tìm trường học theo school_code (mã trường duy nhất).
**Tables involved:** `schools`
**Expected Query Goal:** SELECT * WHERE school_code = '...'.

---

### 47.
**Title:** Học sinh sinh năm cụ thể
**Description:** Lấy danh sách học sinh sinh trong năm 2010 (dựa vào date_of_birth).
**Tables involved:** `students`
**Expected Query Goal:** SELECT * WHERE EXTRACT(YEAR FROM date_of_birth) = 2010.

---

### 48.
**Title:** Lớp học được tạo gần đây (top 5)
**Description:** Lấy 5 lớp học được tạo mới nhất.
**Tables involved:** `classes`
**Expected Query Goal:** SELECT * ORDER BY created_at DESC LIMIT 5.

---

### 49.
**Title:** Chuỗi số thứ tự mã học sinh theo năm
**Description:** Lấy danh sách student_code_sequences sắp xếp theo year DESC.
**Tables involved:** `student_code_sequences`
**Expected Query Goal:** SELECT * ORDER BY year DESC.

---

### 50.
**Title:** Tài khoản có email chứa tên miền cụ thể
**Description:** Tìm tất cả tài khoản có email kết thúc bằng '@school.edu.vn' (ví dụ tra cứu theo domain).
**Tables involved:** `users`
**Expected Query Goal:** SELECT * WHERE email LIKE '%@school.edu.vn'.

---
