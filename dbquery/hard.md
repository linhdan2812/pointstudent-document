# SQL Practice Exercises — Hard (50 bài)

> Cấp độ: **Hard**
> Đặc điểm: Subquery, Window functions, Nested query, Complex joins, Business logic query
> Hệ thống: Quản lý Điểm Học sinh

---

### 1.
**Title:** Tính ĐTB theo công thức hệ số của từng học sinh trong từng phân công bộ môn
**Description:** Tính điểm trung bình có trọng số theo công thức `SUM(value × coefficient) / SUM(coefficient)` cho mỗi cặp (student_id, subject_assignment_id). Chỉ tính các điểm không NULL.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `students`
**Expected Query Goal:** JOIN + GROUP BY student_id, subject_assignment_id + SUM(value * coefficient) / SUM(coefficient) với điều kiện value IS NOT NULL.

---

### 2.
**Title:** Học sinh có ĐTB cao nhất trong mỗi lớp (môn cụ thể)
**Description:** Với một phân công bộ môn cho trước, tìm học sinh có ĐTB cao nhất trong lớp đó. Dùng subquery để lọc MAX ĐTB.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`
**Expected Query Goal:** Subquery tính ĐTB từng học sinh → outer query lấy MAX.

---

### 3.
**Title:** Xếp hạng học sinh theo ĐTB trong từng lớp (window function)
**Description:** Tính ĐTB của mỗi học sinh trong một lớp theo một môn, rồi xếp hạng bằng `RANK()` hoặc `DENSE_RANK()` theo ĐTB giảm dần.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`
**Expected Query Goal:** CTE tính ĐTB → RANK() OVER (PARTITION BY class_id ORDER BY avg DESC).

---

### 4.
**Title:** Top 3 học sinh có ĐTB cao nhất mỗi lớp (window function)
**Description:** Lấy top 3 học sinh có ĐTB cao nhất trong từng lớp (tính theo tất cả môn học). Dùng window function để partition theo class và lấy rank <= 3.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`, `classes`
**Expected Query Goal:** CTE tính ĐTB tổng hợp → DENSE_RANK() OVER (PARTITION BY class_id ORDER BY avg DESC) → WHERE rank <= 3.

---

### 5.
**Title:** Lớp chưa có bất kỳ điểm nào được nhập
**Description:** Tìm các lớp học mà tất cả cột điểm (nếu có) đều trống (value IS NULL hoặc chưa có score_column nào), tức giáo viên chưa nhập điểm cho lớp đó.
**Tables involved:** `classes`, `subject_assignments`, `score_columns`, `scores`
**Expected Query Goal:** NOT EXISTS subquery kiểm tra không có score nào với value IS NOT NULL thuộc lớp đó.

---

### 6.
**Title:** Phân công bộ môn chưa có cột điểm nào (giáo viên chưa thiết lập)
**Description:** Tìm các subject_assignment chưa có score_column nào, kèm tên giáo viên, tên lớp, tên môn học.
**Tables involved:** `subject_assignments`, `score_columns`, `teachers`, `classes`, `subjects`
**Expected Query Goal:** LEFT JOIN score_columns + WHERE score_columns.id IS NULL + JOIN để lấy tên thực thể.

---

### 7.
**Title:** Giáo viên vừa là GVCN vừa là GVBM trong cùng một lớp
**Description:** Tìm các giáo viên có homeroom_teacher_id trong classes đồng thời cũng có teacher_id trong subject_assignments cho cùng class đó.
**Tables involved:** `classes`, `subject_assignments`, `teachers`
**Expected Query Goal:** JOIN classes → subject_assignments WHERE classes.homeroom_teacher_id = subject_assignments.teacher_id AND classes.id = subject_assignments.class_id.

---

### 8.
**Title:** Tính ĐTB tổng hợp của mỗi học sinh qua tất cả môn trong một năm học
**Description:** Với mỗi học sinh trong một năm học, tính ĐTB trung bình tổng hợp: trung bình của các ĐTB từng môn (đã áp công thức hệ số). Chỉ tính môn có ít nhất 1 điểm không NULL.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`, `classes`, `academic_years`
**Expected Query Goal:** CTE đa tầng: tầng 1 tính ĐTB từng môn → tầng 2 AVG các ĐTB môn theo student + academic_year.

---

### 9.
**Title:** Học sinh có ĐTB môn nào đó dưới 5 (học sinh yếu)
**Description:** Tìm học sinh có ĐTB (tính theo hệ số) dưới 5 trong ít nhất một môn học, kèm tên môn và ĐTB đó.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `subjects`, `students`
**Expected Query Goal:** CTE tính ĐTB từng (student, assignment) → lọc WHERE avg < 5.

---

### 10.
**Title:** Học sinh có ĐTB tất cả các môn đều >= 8 (học sinh giỏi)
**Description:** Tìm học sinh mà ĐTB của tất cả môn học (có ít nhất 1 điểm) đều >= 8.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`
**Expected Query Goal:** CTE tính ĐTB từng môn → GROUP BY student → HAVING MIN(avg) >= 8.

---

### 11.
**Title:** Tỷ lệ điểm đã nhập trên tổng số ô điểm cần nhập (fill rate)
**Description:** Với mỗi subject_assignment, tính tỷ lệ phần trăm số điểm đã được nhập (value IS NOT NULL) so với tổng số ô điểm cần nhập (số score_column × số học sinh trong lớp).
**Tables involved:** `subject_assignments`, `score_columns`, `scores`, `class_students`
**Expected Query Goal:** Subquery đếm students trong lớp × count(score_columns) = expected; đếm actual = scores với value IS NOT NULL; tính percentage.

---

### 12.
**Title:** Tỷ lệ cột điểm đã được nhập đủ (tất cả học sinh có điểm)
**Description:** Trong mỗi score_column, đếm xem có bao nhiêu học sinh có điểm (value IS NOT NULL) so với tổng học sinh trong lớp. Cột nào đã nhập đủ 100%?
**Tables involved:** `score_columns`, `scores`, `subject_assignments`, `class_students`
**Expected Query Goal:** Subquery tổng học sinh lớp; COUNT(scores IS NOT NULL) / total × 100.

---

### 13.
**Title:** Tìm phụ huynh có tất cả con đều có ĐTB >= 8
**Description:** Lấy danh sách phụ huynh mà tất cả con của họ đều đạt ĐTB tổng hợp >= 8 trong năm học gần nhất.
**Tables involved:** `parents`, `students`, `class_students`, `scores`, `score_columns`, `subject_assignments`
**Expected Query Goal:** CTE tính ĐTB tổng của từng học sinh → JOIN về parents → HAVING MIN(avg) >= 8.

---

### 14.
**Title:** ĐTB so sánh với trung bình của lớp (above/below average)
**Description:** Với mỗi học sinh trong một lớp - môn cụ thể, tính ĐTB của học sinh đó và so sánh với ĐTB trung bình của cả lớp trong môn đó. Hiển thị cột "so_sanh" = 'tren' hoặc 'duoi'.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`
**Expected Query Goal:** Window function AVG() OVER (PARTITION BY subject_assignment_id) → so sánh individual avg với window avg.

---

### 15.
**Title:** Giáo viên nào chưa nhập điểm cho bất kỳ học sinh nào
**Description:** Tìm giáo viên có subject_assignment nhưng không có bất kỳ score nào (value IS NOT NULL) được nhập trong các score_column thuộc phân công đó.
**Tables involved:** `teachers`, `subject_assignments`, `score_columns`, `scores`
**Expected Query Goal:** LEFT JOIN hoặc NOT EXISTS subquery kiểm tra scores.value IS NOT NULL.

---

### 16.
**Title:** Nhận xét bị lỡ lịch gửi (scheduled_at đã qua nhưng vẫn là 'scheduled')
**Description:** Tìm các comment có scheduled_at < NOW() nhưng status vẫn là 'scheduled', kèm thông tin giáo viên, học sinh, lớp.
**Tables involved:** `comments`, `teachers`, `students`, `classes`
**Expected Query Goal:** JOIN + WHERE scheduled_at < NOW() AND status = 'scheduled'.

---

### 17.
**Title:** Hiệu suất gửi thông báo (tỷ lệ thông báo đến tay người nhận)
**Description:** Theo từng comment, tính xem có bao nhiêu notification liên quan đã được gửi thành công (is_sent = true). Hiển thị comment_id, tổng notification, số đã gửi, tỷ lệ %.
**Tables involved:** `comments`, `notifications`
**Expected Query Goal:** GROUP BY comment_id + COUNT + SUM(CASE WHEN is_sent THEN 1 ELSE 0 END) → tỷ lệ.

---

### 18.
**Title:** Học sinh có điểm tuyệt đối 10 trong ít nhất một cột điểm
**Description:** Tìm học sinh có value = 10 trong ít nhất một score, kèm tên cột điểm và tên môn học.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `subjects`, `students`
**Expected Query Goal:** JOIN + WHERE value = 10.

---

### 19.
**Title:** Phân phối điểm theo khoảng (0-4, 5-6, 7-8, 9-10) của từng môn
**Description:** Đếm số điểm rơi vào từng khoảng điểm cho mỗi subject_assignment. Dùng CASE WHEN để phân loại.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `subjects`
**Expected Query Goal:** JOIN + CASE WHEN value < 5 THEN '0-4' WHEN value < 7 THEN '5-6' WHEN value < 9 THEN '7-8' ELSE '9-10' END + GROUP BY subject_assignment_id, range.

---

### 20.
**Title:** Môn học có điểm trung bình thấp nhất (môn khó nhất) theo từng lớp
**Description:** Với mỗi lớp, tìm môn học có ĐTB (tính theo hệ số) thấp nhất. Dùng subquery hoặc window function để lấy min per class.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `classes`, `subjects`
**Expected Query Goal:** CTE tính ĐTB từng assignment → RANK() OVER (PARTITION BY class_id ORDER BY avg ASC) → WHERE rank = 1.

---

### 21.
**Title:** Môn học có điểm trung bình cao nhất (môn dễ nhất) theo từng lớp
**Description:** Ngược lại bài 20 — tìm môn có ĐTB cao nhất trong từng lớp.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `classes`, `subjects`
**Expected Query Goal:** CTE tính ĐTB từng assignment → RANK() OVER (PARTITION BY class_id ORDER BY avg DESC) → WHERE rank = 1.

---

### 22.
**Title:** Chênh lệch điểm cao nhất và thấp nhất trong lớp theo từng môn
**Description:** Với mỗi (class, subject), tính MAX(ĐTB học sinh) - MIN(ĐTB học sinh) để xem mức độ chênh lệch trong lớp.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `students`
**Expected Query Goal:** CTE tính ĐTB từng học sinh → GROUP BY assignment → MAX(avg) - MIN(avg).

---

### 23.
**Title:** Học sinh cùng phụ huynh trong cùng một lớp
**Description:** Tìm các cặp học sinh có cùng parent_id và cùng học một lớp (anh/chị/em đồng lớp).
**Tables involved:** `students`, `class_students`
**Expected Query Goal:** Self-join class_students ON class_id = class_id → JOIN students ON student_id để lọc cùng parent_id.

---

### 24.
**Title:** Tiến độ nhập điểm của từng giáo viên bộ môn
**Description:** Với mỗi giáo viên, tính tổng số ô điểm cần nhập và số ô đã nhập (value IS NOT NULL) trong tất cả phân công của họ (năm học đang diễn ra).
**Tables involved:** `subject_assignments`, `score_columns`, `scores`, `class_students`, `teachers`, `academic_years`, `classes`
**Expected Query Goal:** Subquery tính expected = SUM(count_students × count_columns); actual = COUNT(scores IS NOT NULL); GROUP BY teacher_id.

---

### 25.
**Title:** Học sinh có ĐTB cải thiện giữa các năm học
**Description:** So sánh ĐTB tổng hợp của học sinh qua các năm học (từ class_students). Tìm học sinh mà ĐTB năm sau cao hơn năm trước.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `classes`, `academic_years`, `students`
**Expected Query Goal:** CTE tính ĐTB tổng hợp theo (student, academic_year) → LAG() OVER (PARTITION BY student_id ORDER BY start_date) → lọc current_avg > prev_avg.

---

### 26.
**Title:** Xếp hạng trường theo ĐTB học sinh toàn trường (năm học đang diễn ra)
**Description:** Tính ĐTB tổng hợp của tất cả học sinh trong từng trường trong năm học in_progress, rồi xếp hạng trường theo ĐTB đó.
**Tables involved:** `schools`, `students`, `class_students`, `scores`, `score_columns`, `subject_assignments`, `classes`, `academic_years`
**Expected Query Goal:** CTE tính ĐTB từng học sinh → AVG theo school → RANK() OVER ORDER BY school_avg DESC.

---

### 27.
**Title:** Học sinh chưa đăng nhập lần nào (tài khoản chưa dùng)
**Description:** Tìm học sinh có tài khoản (users) nhưng chưa từng tạo password_reset_token (proxy cho hoạt động tài khoản) và cũng chưa có notification nào được gửi đến.
**Tables involved:** `students`, `users`, `password_reset_tokens`, `notifications`
**Expected Query Goal:** LEFT JOIN password_reset_tokens + LEFT JOIN notifications → WHERE cả hai IS NULL.

---

### 28.
**Title:** Giáo viên dạy nhiều trường nhất (phát hiện dữ liệu bất thường)
**Description:** Tìm giáo viên có school_id khác với school_id của lớp mà họ được phân công dạy (subject_assignment). Đây là trường hợp bất thường trong dữ liệu.
**Tables involved:** `subject_assignments`, `teachers`, `classes`, `academic_years`
**Expected Query Goal:** JOIN + WHERE teachers.school_id != academic_years.school_id (qua classes → academic_years).

---

### 29.
**Title:** Lớp có tỷ lệ học sinh yếu (ĐTB < 5) cao nhất
**Description:** Với mỗi lớp, tính tỷ lệ % học sinh có ít nhất 1 môn ĐTB < 5. Sắp xếp theo tỷ lệ giảm dần.
**Tables involved:** `class_students`, `scores`, `score_columns`, `subject_assignments`, `classes`, `students`
**Expected Query Goal:** CTE tính ĐTB từng (student, assignment) → đánh dấu học sinh yếu → GROUP BY class → tính tỷ lệ.

---

### 30.
**Title:** Giáo viên có số nhận xét đã gửi nhiều nhất trong 30 ngày gần đây
**Description:** Tìm top 5 giáo viên có nhiều comment status = 'sent' nhất trong 30 ngày qua.
**Tables involved:** `comments`, `teachers`
**Expected Query Goal:** JOIN + WHERE status = 'sent' AND created_at >= NOW() - INTERVAL '30 days' + GROUP BY teacher_id + COUNT ORDER BY count DESC LIMIT 5.

---

### 31.
**Title:** Học sinh không có điểm trong bất kỳ môn nào (dù đã xếp lớp)
**Description:** Tìm học sinh đã được xếp vào lớp (có trong class_students) nhưng không có bất kỳ score nào với value IS NOT NULL thuộc lớp đó.
**Tables involved:** `class_students`, `scores`, `score_columns`, `subject_assignments`, `students`
**Expected Query Goal:** LEFT JOIN dây chuyền + WHERE scores.value IS NOT NULL cuối cùng = NULL → NOT EXISTS.

---

### 32.
**Title:** Số ngày trung bình từ khi tạo comment đến khi gửi
**Description:** Tính khoảng thời gian trung bình (tính bằng ngày) từ created_at đến scheduled_at cho các comment đã được gửi (status = 'sent').
**Tables involved:** `comments`
**Expected Query Goal:** WHERE status = 'sent' + AVG(EXTRACT(EPOCH FROM (scheduled_at - created_at)) / 86400).

---

### 33.
**Title:** Cột điểm có điểm trung bình thấp nhất trong mỗi phân công (hệ số bao nhiêu thì khó nhất)
**Description:** Với mỗi subject_assignment, tìm score_column có AVG(value) thấp nhất, kèm hệ số của cột đó.
**Tables involved:** `score_columns`, `scores`, `subject_assignments`
**Expected Query Goal:** CTE tính AVG từng score_column → RANK() OVER (PARTITION BY subject_assignment_id ORDER BY avg ASC) → WHERE rank = 1.

---

### 34.
**Title:** Học sinh học cùng lớp qua nhiều năm liên tiếp (lưu ban?)
**Description:** Tìm học sinh có class_students trong ít nhất 2 năm học thuộc cùng một academic_year name (tên lớp giống nhau), gợi ý khả năng lưu ban.
**Tables involved:** `class_students`, `classes`, `academic_years`, `students`
**Expected Query Goal:** Self-join hoặc GROUP BY student_id + classes.name → HAVING COUNT(DISTINCT academic_year_id) > 1.

---

### 35.
**Title:** Tổng số điểm đã nhập theo từng tháng trong năm học
**Description:** Thống kê số bản ghi score (value IS NOT NULL) được tạo/cập nhật theo từng tháng.
**Tables involved:** `scores`
**Expected Query Goal:** WHERE value IS NOT NULL + GROUP BY DATE_TRUNC('month', updated_at) + COUNT ORDER BY month.

---

### 36.
**Title:** Trường học có tốc độ tăng trưởng học sinh cao nhất giữa các năm học
**Description:** So sánh số học sinh được xếp lớp giữa năm học trước và năm học sau cho từng trường. Tính delta và xếp hạng.
**Tables involved:** `class_students`, `classes`, `academic_years`, `schools`
**Expected Query Goal:** CTE đếm học sinh theo (school, academic_year) → LAG() OVER (PARTITION BY school_id ORDER BY start_date) → tính delta → ORDER BY delta DESC.

---

### 37.
**Title:** Học sinh có ít nhất 1 điểm ở tất cả các môn học trong lớp
**Description:** Tìm học sinh mà ở tất cả subject_assignment của lớp họ, đều có ít nhất 1 score với value IS NOT NULL (đã được nhập đủ điểm).
**Tables involved:** `class_students`, `subject_assignments`, `score_columns`, `scores`, `students`
**Expected Query Goal:** Đếm số subject_assignment của lớp = số môn học sinh có ít nhất 1 điểm → HAVING equal.

---

### 38.
**Title:** Tìm lớp có nhiều nhận xét "bị lỡ" nhất (scheduled qua hạn)
**Description:** Đếm số comment có scheduled_at < NOW() AND status = 'scheduled' theo từng lớp. Sắp xếp giảm dần.
**Tables involved:** `comments`, `classes`
**Expected Query Goal:** JOIN + WHERE scheduled_at < NOW() AND status = 'scheduled' + GROUP BY class_id + COUNT ORDER BY count DESC.

---

### 39.
**Title:** Phân tích ĐTB theo hệ số cột điểm (hệ số cao → điểm trung bình cao hay thấp hơn?)
**Description:** Tính AVG(value) theo từng coefficient để phân tích mối quan hệ giữa hệ số và điểm số trung bình.
**Tables involved:** `scores`, `score_columns`
**Expected Query Goal:** JOIN + WHERE value IS NOT NULL + GROUP BY coefficient + AVG(value) ORDER BY coefficient.

---

### 40.
**Title:** Học sinh có ĐTB cao nhất toàn trường trong năm học đang diễn ra
**Description:** Tìm học sinh có ĐTB tổng hợp (trung bình các môn có hệ số) cao nhất trong toàn trường, năm học in_progress.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `classes`, `academic_years`, `students`, `schools`
**Expected Query Goal:** CTE tính ĐTB từng học sinh → RANK() OVER (PARTITION BY school_id ORDER BY avg DESC) → WHERE rank = 1.

---

### 41.
**Title:** Báo cáo tổng hợp lớp: số học sinh, số môn, số điểm đã nhập, ĐTB lớp
**Description:** Tạo báo cáo đầy đủ cho mỗi lớp trong năm học đang diễn ra: tên lớp, số HS, số phân công BM, tổng điểm đã nhập, ĐTB toàn lớp (tính theo hệ số).
**Tables involved:** `classes`, `academic_years`, `class_students`, `subject_assignments`, `score_columns`, `scores`
**Expected Query Goal:** CTE đa tầng: đếm HS, đếm phân công, tính ĐTB; JOIN tất cả vào classes.

---

### 42.
**Title:** Giáo viên chưa tạo cột điểm nào cho lớp bộ môn đang dạy (năm in_progress)
**Description:** Tìm giáo viên có subject_assignment trong năm học đang diễn ra nhưng chưa tạo bất kỳ score_column nào.
**Tables involved:** `subject_assignments`, `score_columns`, `teachers`, `classes`, `academic_years`
**Expected Query Goal:** LEFT JOIN score_columns + WHERE score_columns.id IS NULL + JOIN academic_years WHERE status = 'in_progress'.

---

### 43.
**Title:** Xếp hạng học sinh theo ĐTB trong từng năm học (xuyên suốt các năm)
**Description:** Tính ĐTB tổng hợp của mỗi học sinh trong từng năm học họ học, và xếp hạng trong toàn trường cho năm đó.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `class_students`, `classes`, `academic_years`, `students`
**Expected Query Goal:** CTE tính ĐTB theo (student, academic_year) → RANK() OVER (PARTITION BY academic_year_id ORDER BY avg DESC).

---

### 44.
**Title:** Tỷ lệ hoàn thành nhập điểm theo từng trường
**Description:** Với mỗi trường (trong năm học in_progress), tính tỷ lệ % tổng ô điểm đã nhập / tổng ô điểm cần nhập (score_columns × class_students).
**Tables involved:** `schools`, `academic_years`, `classes`, `class_students`, `subject_assignments`, `score_columns`, `scores`
**Expected Query Goal:** CTE tính expected (school) và actual (school) → percentage = actual / expected * 100.

---

### 45.
**Title:** Học sinh có ĐTB môn giảm so với kỳ trước (trong cùng năm học)
**Description:** Giả sử có thể phân biệt các cột điểm theo order, hãy tính xu hướng điểm của học sinh: so sánh điểm cột sau với cột trước trong cùng phân công. Tìm học sinh mà điểm gần nhất thấp hơn điểm trước đó.
**Tables involved:** `scores`, `score_columns`, `subject_assignments`, `students`
**Expected Query Goal:** LAG() OVER (PARTITION BY student_id, subject_assignment_id ORDER BY score_columns.order) → WHERE current < previous.

---

### 46.
**Title:** Danh sách token reset password còn hiệu lực và chưa dùng theo từng user
**Description:** Tìm user đang có nhiều hơn 1 token chưa dùng (used_at IS NULL) và còn hạn (expires_at > NOW()). Đây là trường hợp bất thường (user request nhiều lần).
**Tables involved:** `password_reset_tokens`, `users`
**Expected Query Goal:** JOIN + WHERE used_at IS NULL AND expires_at > NOW() + GROUP BY user_id + HAVING COUNT > 1.

---

### 47.
**Title:** Tìm học sinh bị thiếu tài khoản (student không có user)
**Description:** Tìm bản ghi trong bảng students có user_id nhưng không tồn tại bản ghi tương ứng trong bảng users (kiểm tra tính toàn vẹn dữ liệu).
**Tables involved:** `students`, `users`
**Expected Query Goal:** LEFT JOIN users + WHERE users.id IS NULL (orphan records check).

---

### 48.
**Title:** Học sinh trong lớp kèm ĐTB từng môn và xếp hạng trong lớp
**Description:** Với một lớp cụ thể, lấy toàn bộ học sinh, ĐTB từng môn, và xếp hạng của từng học sinh theo từng môn trong lớp.
**Tables involved:** `class_students`, `students`, `subject_assignments`, `score_columns`, `scores`, `subjects`
**Expected Query Goal:** CTE tính ĐTB từng (student, assignment) → RANK() OVER (PARTITION BY subject_assignment_id ORDER BY avg DESC).

---

### 49.
**Title:** Báo cáo hoạt động nhận xét: số comment theo tháng, theo trạng thái, theo trường
**Description:** Thống kê số comment được tạo theo từng tháng, phân theo status (sent/scheduled), cho từng trường (dựa vào class → academic_year → school).
**Tables involved:** `comments`, `classes`, `academic_years`, `schools`
**Expected Query Goal:** JOIN + GROUP BY DATE_TRUNC('month', comments.created_at), status, school_id + COUNT.

---

### 50.
**Title:** Dashboard tổng hợp trường: top học sinh giỏi, môn khó nhất, lớp có ĐTB cao nhất
**Description:** Viết một query (hoặc CTE đa tầng) trả về 3 thông tin trong một năm học cụ thể: (1) top 5 học sinh toàn trường theo ĐTB, (2) môn học có ĐTB thấp nhất, (3) lớp có ĐTB cao nhất. Đây là bài tổng hợp kiến thức hard.
**Tables involved:** `schools`, `academic_years`, `classes`, `class_students`, `students`, `subject_assignments`, `subjects`, `score_columns`, `scores`
**Expected Query Goal:** Nhiều CTE: `student_avg` → tính ĐTB từng HS; `subject_avg` → ĐTB từng môn; `class_avg` → ĐTB từng lớp; cuối cùng UNION hoặc 3 SELECT riêng biệt trong cùng script.

---
