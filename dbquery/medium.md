# SQL Practice Exercises — Medium (50 bài)

> Cấp độ: **Medium**
> Đặc điểm: JOIN nhiều bảng, GROUP BY, HAVING, Aggregate functions (COUNT, SUM, AVG, MAX, MIN)
> Hệ thống: Quản lý Điểm Học sinh

---

### 1.
**Title:** Danh sách học sinh kèm tên lớp hiện tại
**Description:** Lấy danh sách học sinh cùng với tên lớp họ đang theo học (qua class_students và classes).
**Tables involved:** `students`, `class_students`, `classes`
**Expected Query Goal:** JOIN students → class_students → classes, SELECT student info + class name.

---

### 2.
**Title:** Số lượng học sinh theo từng lớp
**Description:** Đếm tổng số học sinh trong mỗi lớp. Sắp xếp theo số lượng giảm dần.
**Tables involved:** `class_students`, `classes`
**Expected Query Goal:** JOIN + GROUP BY class_id + COUNT(student_id) ORDER BY count DESC.

---

### 3.
**Title:** Giáo viên và lớp chủ nhiệm của họ
**Description:** Lấy danh sách giáo viên kèm tên lớp mà họ đang chủ nhiệm.
**Tables involved:** `teachers`, `classes`
**Expected Query Goal:** JOIN teachers → classes ON homeroom_teacher_id = teachers.id.

---

### 4.
**Title:** Học sinh kèm thông tin phụ huynh
**Description:** Lấy danh sách học sinh cùng với tên cha, tên mẹ và số điện thoại phụ huynh.
**Tables involved:** `students`, `parents`
**Expected Query Goal:** JOIN students → parents ON parent_id = parents.id.

---

### 5.
**Title:** Số lượng môn học theo từng năm học
**Description:** Đếm số môn học trong mỗi năm học.
**Tables involved:** `subjects`, `academic_years`
**Expected Query Goal:** JOIN + GROUP BY academic_year_id + COUNT(subjects.id).

---

### 6.
**Title:** Lớp học kèm tên năm học và giáo viên chủ nhiệm
**Description:** Lấy thông tin lớp học đầy đủ: tên lớp, tên năm học, họ tên GVCN.
**Tables involved:** `classes`, `academic_years`, `teachers`
**Expected Query Goal:** JOIN classes → academic_years + teachers, SELECT class.name, academic_year.name, teacher.full_name.

---

### 7.
**Title:** Số lượng cột điểm theo từng phân công bộ môn
**Description:** Đếm số cột điểm (score_columns) trong mỗi subject_assignment.
**Tables involved:** `score_columns`, `subject_assignments`
**Expected Query Goal:** GROUP BY subject_assignment_id + COUNT(score_columns.id).

---

### 8.
**Title:** Giáo viên dạy bộ môn trong từng lớp
**Description:** Lấy danh sách phân công: tên lớp, tên môn học, họ tên giáo viên bộ môn.
**Tables involved:** `subject_assignments`, `classes`, `subjects`, `teachers`
**Expected Query Goal:** JOIN 3 bảng từ subject_assignments.

---

### 9.
**Title:** Học sinh kèm tên trường
**Description:** Lấy danh sách học sinh cùng với tên trường học mà họ theo học.
**Tables involved:** `students`, `schools`
**Expected Query Goal:** JOIN students → schools ON school_id = schools.id.

---

### 10.
**Title:** Số lượng học sinh theo từng trường
**Description:** Đếm tổng số học sinh trong mỗi trường, sắp xếp theo số lượng giảm dần.
**Tables involved:** `students`, `schools`
**Expected Query Goal:** JOIN + GROUP BY school_id + COUNT, ORDER BY count DESC.

---

### 11.
**Title:** Số lượng giáo viên theo từng trường
**Description:** Đếm tổng số giáo viên (active) trong mỗi trường.
**Tables involved:** `teachers`, `schools`
**Expected Query Goal:** JOIN + WHERE work_status = 'active' + GROUP BY school_id + COUNT.

---

### 12.
**Title:** Số lượng năm học theo từng trường
**Description:** Đếm số năm học đã có trong mỗi trường.
**Tables involved:** `academic_years`, `schools`
**Expected Query Goal:** JOIN + GROUP BY school_id + COUNT.

---

### 13.
**Title:** Lớp học trong năm học đang diễn ra kèm số học sinh
**Description:** Lấy các lớp thuộc năm học in_progress cùng số học sinh trong mỗi lớp.
**Tables involved:** `classes`, `academic_years`, `class_students`
**Expected Query Goal:** JOIN + WHERE academic_years.status = 'in_progress' + GROUP BY + COUNT.

---

### 14. (bỏ qua)
**Title:** Thông báo kèm địa chỉ email người nhận và nội dung comment
**Description:** Lấy thông tin notification với email người nhận và content của comment liên quan.
**Tables involved:** `notifications`, `comments`
**Expected Query Goal:** JOIN notifications → comments ON comment_id.

---

### 15.
**Title:** Số nhận xét theo từng học sinh
**Description:** Đếm số comment của mỗi học sinh, chỉ tính comment status = 'sent'.
**Tables involved:** `comments`, `students`
**Expected Query Goal:** JOIN + WHERE status = 'sent' + GROUP BY student_id + COUNT.

---

### 16.
**Title:** Số nhận xét theo từng giáo viên
**Description:** Đếm số comment mà mỗi giáo viên đã tạo.
**Tables involved:** `comments`, `teachers`
**Expected Query Goal:** JOIN + GROUP BY teacher_id + COUNT.

---

### 17. (bỏ qua vì trong 1 năm chủ nhiệm chỉ 1 lớp)
**Title:** Giáo viên chủ nhiệm nhiều lớp nhất (trong năm học in_progress)
**Description:** Đếm số lớp chủ nhiệm mỗi giáo viên đang phụ trách trong năm học đang diễn ra.
**Tables involved:** `teachers`, `classes`, `academic_years`
**Expected Query Goal:** JOIN + WHERE status = 'in_progress' + GROUP BY teacher_id + COUNT + ORDER BY count DESC.

---

### 18. (bỏ qua)
**Title:** Số lượng thông báo chưa đọc theo từng người dùng
**Description:** Đếm notification chưa đọc (is_read = false) của từng user.
**Tables involved:** `notifications`, `users`
**Expected Query Goal:** JOIN + WHERE is_read = false + GROUP BY user_id + COUNT.

---

### 19.
**Title:** Môn học được phân công trong mỗi lớp
**Description:** Lấy danh sách các môn học đang được dạy trong từng lớp (năm học đang diễn ra).
**Tables involved:** `subject_assignments`, `classes`, `subjects`, `academic_years`
**Expected Query Goal:** JOIN + WHERE status = 'in_progress'.

---

### 20.
**Title:** Điểm trung bình của tất cả học sinh theo từng cột điểm
**Description:** Tính điểm trung bình (AVG) của mỗi score_column, chỉ tính các điểm không NULL.
**Tables involved:** `scores`, `score_columns`
**Expected Query Goal:** GROUP BY score_column_id + AVG(value) WHERE value IS NOT NULL.

---

### 21.
**Title:** Điểm cao nhất và thấp nhất theo từng cột điểm
**Description:** Tìm MAX và MIN value trong mỗi score_column.
**Tables involved:** `scores`, `score_columns`
**Expected Query Goal:** GROUP BY score_column_id + MAX(value) + MIN(value).

---

### 22.
**Title:** Số cột điểm theo từng hệ số (coefficient)
**Description:** Đếm số cột điểm theo từng loại hệ số (1, 2, 3...).
**Tables involved:** `score_columns`
**Expected Query Goal:** GROUP BY coefficient + COUNT.

---

### 23.
**Title:** Học sinh trong lớp cụ thể kèm thông tin phụ huynh
**Description:** Lấy danh sách học sinh trong một lớp cùng thông tin liên lạc phụ huynh.
**Tables involved:** `class_students`, `students`, `parents`
**Expected Query Goal:** JOIN + WHERE class_id = '...' + JOIN parents.

---

### 24.
**Title:** Số học sinh theo giới tính trong từng trường
**Description:** Đếm học sinh nam/nữ trong mỗi trường.
**Tables involved:** `students`, `schools`
**Expected Query Goal:** JOIN + GROUP BY school_id, gender + COUNT.

---

### 25.
**Title:** Số học sinh theo giới tính trong từng lớp
**Description:** Đếm học sinh nam/nữ trong mỗi lớp.
**Tables involved:** `class_students`, `students`, `classes`
**Expected Query Goal:** JOIN + GROUP BY class_id, gender + COUNT.

---

### 26.
**Title:** Lớp có nhiều học sinh hơn 30 em
**Description:** Lấy các lớp có số lượng học sinh > 30.
**Tables involved:** `class_students`, `classes`
**Expected Query Goal:** GROUP BY class_id + COUNT HAVING COUNT > 30.

---

### 27.
**Title:** Giáo viên dạy từ 3 môn trở lên (trong năm học in_progress)
**Description:** Tìm giáo viên có số phân công bộ môn >= 3 trong năm học đang diễn ra.
**Tables involved:** `subject_assignments`, `classes`, `academic_years`, `teachers`
**Expected Query Goal:** JOIN + WHERE status = 'in_progress' + GROUP BY teacher_id + COUNT HAVING COUNT >= 3.

---

### 28.
**Title:** Phụ huynh có nhiều hơn 1 con trong hệ thống
**Description:** Tìm phụ huynh có từ 2 con trở lên.
**Tables involved:** `students`, `parents`
**Expected Query Goal:** GROUP BY parent_id + COUNT HAVING COUNT >= 2.

---

### 29.
**Title:** Tổng số điểm đã nhập theo từng phân công bộ môn
**Description:** Đếm số bản ghi scores có value IS NOT NULL theo từng subject_assignment.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`
**Expected Query Goal:** JOIN + WHERE value IS NOT NULL + GROUP BY subject_assignment_id + COUNT.

---

### 30.
**Title:** Năm học có số lớp học nhiều nhất
**Description:** Đếm số lớp trong mỗi năm học, sắp xếp theo số lượng lớp giảm dần.
**Tables involved:** `classes`, `academic_years`
**Expected Query Goal:** JOIN + GROUP BY academic_year_id + COUNT ORDER BY count DESC.

---

### 31.
**Title:** Lớp học chưa có học sinh nào
**Description:** Tìm các lớp mà không có bất kỳ học sinh nào trong bảng class_students.
**Tables involved:** `classes`, `class_students`
**Expected Query Goal:** LEFT JOIN + WHERE class_students.id IS NULL (hoặc HAVING COUNT = 0).

---

### 32.
**Title:** Phân công bộ môn chưa có cột điểm nào
**Description:** Tìm các subject_assignment chưa có score_column nào được tạo.
**Tables involved:** `subject_assignments`, `score_columns`
**Expected Query Goal:** LEFT JOIN + WHERE score_columns.id IS NULL.

---

### 33.
**Title:** Học sinh chưa có nhận xét nào
**Description:** Tìm các học sinh chưa từng có comment.
**Tables involved:** `students`, `comments`
**Expected Query Goal:** LEFT JOIN + WHERE comments.id IS NULL.

---

### 34.
**Title:** Thông báo kèm thông tin người nhận (user email)
**Description:** Lấy notification cùng email của user nhận thông báo.
**Tables involved:** `notifications`, `users`
**Expected Query Goal:** JOIN notifications → users ON user_id.

---

### 35.
**Title:** Số thông báo gửi thành công theo từng ngày
**Description:** Đếm notification có is_sent = true theo từng ngày (group by DATE(sent_at)).
**Tables involved:** `notifications`
**Expected Query Goal:** WHERE is_sent = true + GROUP BY DATE(sent_at) + COUNT.

---

### 36.
**Title:** Danh sách học sinh kèm email đăng nhập
**Description:** Lấy thông tin học sinh và địa chỉ email tài khoản (từ bảng users).
**Tables involved:** `students`, `users`
**Expected Query Goal:** JOIN students → users ON user_id.

---

### 37.
**Title:** Danh sách giáo viên kèm email đăng nhập
**Description:** Lấy thông tin giáo viên và địa chỉ email tài khoản.
**Tables involved:** `teachers`, `users`
**Expected Query Goal:** JOIN teachers → users ON user_id.

---

### 38.
**Title:** Điểm trung bình của mỗi học sinh trong một phân công bộ môn
**Description:** Tính AVG(value) cho mỗi student trong mỗi subject_assignment. Chỉ tính điểm không NULL.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `students`
**Expected Query Goal:** JOIN + WHERE value IS NOT NULL + GROUP BY subject_assignment_id, student_id + AVG.

---

### 39.
**Title:** Tổng số điểm đã nhập theo từng trường (trong năm học đang diễn ra)
**Description:** Đếm tổng bản ghi scores không NULL theo từng school, chỉ năm học in_progress.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `classes`, `academic_years`, `schools`
**Expected Query Goal:** JOIN nhiều bảng + WHERE status = 'in_progress' AND value IS NOT NULL + GROUP BY school_id.

---

### 40.
**Title:** Học sinh tham gia nhiều năm học nhất
**Description:** Đếm số năm học mỗi học sinh đã từng được xếp lớp (dựa trên class_students).
**Tables involved:** `class_students`, `students`
**Expected Query Goal:** JOIN + GROUP BY student_id + COUNT(DISTINCT academic_year_id) ORDER BY count DESC.

---

### 41.
**Title:** Số comment theo trạng thái trong từng lớp
**Description:** Đếm số comment theo status (sent/scheduled) trong mỗi lớp.
**Tables involved:** `comments`, `classes`
**Expected Query Goal:** JOIN + GROUP BY class_id, status + COUNT.

---

### 42.
**Title:** Giáo viên và số môn học họ dạy theo từng năm học
**Description:** Lấy danh sách giáo viên, tên năm học, và số môn học họ được phân công.
**Tables involved:** `subject_assignments`, `teachers`, `classes`, `academic_years`
**Expected Query Goal:** JOIN + GROUP BY teacher_id, academic_year_id + COUNT.

---

### 43.
**Title:** Lớp học kèm số phân công bộ môn
**Description:** Lấy tên lớp và đếm số phân công bộ môn (subject_assignments) trong lớp đó.
**Tables involved:** `classes`, `subject_assignments`
**Expected Query Goal:** LEFT JOIN + GROUP BY class_id + COUNT.

---

### 44.
**Title:** Học sinh kèm lớp hiện tại và năm học
**Description:** Lấy thông tin học sinh, tên lớp, tên năm học từ lần đăng ký gần nhất.
**Tables involved:** `students`, `class_students`, `classes`, `academic_years`
**Expected Query Goal:** JOIN 3 bảng + ORDER BY academic_years.start_date DESC.

---

### 45.
**Title:** Nhận xét kèm tên học sinh và tên lớp
**Description:** Lấy nội dung nhận xét cùng full_name học sinh và tên lớp.
**Tables involved:** `comments`, `students`, `classes`
**Expected Query Goal:** JOIN comments → students, classes.

---

### 46.
**Title:** Nhận xét kèm tên giáo viên tạo
**Description:** Lấy nhận xét cùng với họ tên giáo viên đã tạo nhận xét đó.
**Tables involved:** `comments`, `teachers`
**Expected Query Goal:** JOIN comments → teachers ON teacher_id.

---

### 47.
**Title:** Học sinh chưa có điểm nào được nhập
**Description:** Tìm học sinh có trong class_students nhưng không có bất kỳ bản ghi score nào (value IS NOT NULL).
**Tables involved:** `students`, `scores`
**Expected Query Goal:** LEFT JOIN + WHERE scores.id IS NULL hoặc HAVING COUNT = 0.

---

### 48.
**Title:** Trường học kèm số lượng giáo viên active và resigned
**Description:** Đếm số giáo viên theo từng work_status trong mỗi trường.
**Tables involved:** `teachers`, `schools`
**Expected Query Goal:** JOIN + GROUP BY school_id, work_status + COUNT.

---

### 49.
**Title:** Số chuỗi mã học sinh theo từng năm tuyển sinh
**Description:** Lấy dữ liệu từ student_code_sequences, join với schools để lấy tên trường.
**Tables involved:** `student_code_sequences`, `schools`
**Expected Query Goal:** JOIN + SELECT school.name, year, last_seq ORDER BY year DESC.

---

### 50.
**Title:** Lớp học có ít nhất 1 nhận xét đã gửi (sent)
**Description:** Tìm các lớp đã có ít nhất 1 comment có status = 'sent'.
**Tables involved:** `comments`, `classes`
**Expected Query Goal:** JOIN + WHERE status = 'sent' + GROUP BY class_id HAVING COUNT >= 1.

---
