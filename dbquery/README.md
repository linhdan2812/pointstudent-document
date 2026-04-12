# SQL Practice Exercises — Hệ thống Quản lý Điểm Học sinh

Bộ bài tập SQL gồm **150 bài** được thiết kế dựa trên schema thực tế của hệ thống.

## Cấu trúc

| File | Cấp độ | Số bài | Đặc điểm |
|------|--------|--------|-----------|
| [easy.md](./easy.md) | Easy | 50 | SELECT cơ bản, WHERE, ORDER BY, LIMIT — 1 bảng |
| [medium.md](./medium.md) | Medium | 50 | JOIN nhiều bảng, GROUP BY, HAVING, Aggregate functions |
| [hard.md](./hard.md) | Hard | 50 | Subquery, Window functions, CTE, Business logic query |

## Sơ đồ bảng (tham khảo nhanh)

```
users ──────────── teachers ──── classes (homeroom_teacher_id)
  │                    │              │
  ├── students          └── subject_assignments ── score_columns ── scores
  │       │                      │
  └── parents            subjects (via academic_years)
        │
    students (parent_id)

classes ──── academic_years ──── schools
   │
class_students (student_id, class_id, academic_year_id)

comments (teacher_id, student_id, class_id) ──── notifications (user_id)

password_reset_tokens (user_id)
student_code_sequences (school_id, year)
```

## Các bảng chính

| Bảng | Mô tả |
|------|-------|
| `users` | Tài khoản đăng nhập (role: admin_system, admin_school, teacher, parent, student) |
| `schools` | Trường học |
| `academic_years` | Năm học (status: preparing / in_progress / completed) |
| `subjects` | Môn học theo năm học |
| `teachers` | Giáo viên (work_status: active / resigned) |
| `parents` | Phụ huynh |
| `students` | Học sinh (study_status: studying / dropped_out) |
| `classes` | Lớp học (có homeroom_teacher_id) |
| `class_students` | Học sinh thuộc lớp (BR: 1 HS / 1 lớp / năm học) |
| `subject_assignments` | Phân công GVBM cho lớp + môn |
| `score_columns` | Cột điểm (coefficient: hệ số 1/2/3...) |
| `scores` | Điểm số (value NULL = chưa nhập; thang 0–10) |
| `comments` | Nhận xét học sinh (status: scheduled / sent) |
| `notifications` | Thông báo email gửi PH/HS |
| `password_reset_tokens` | Token đặt lại mật khẩu (hiệu lực 15 phút) |
| `student_code_sequences` | Sequence tạo mã học sinh |

## Công thức quan trọng

```sql
-- ĐTB có trọng số (FR-015)
SUM(value * coefficient) / SUM(coefficient)
-- Chỉ tính các điểm value IS NOT NULL
```

## Enum quan trọng

```sql
Role:              admin_system | admin_school | teacher | parent | student
AcademicYearStatus: preparing | in_progress | completed
Gender:            male | female | other
WorkStatus:        active | resigned
StudyStatus:       studying | dropped_out
CommentStatus:     scheduled | sent
```
