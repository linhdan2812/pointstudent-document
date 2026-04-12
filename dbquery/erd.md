# Database Design — Hệ thống Quản lý Điểm Học sinh

> ERD version 1.1 | PostgreSQL | Prisma ORM

---

## ERD — Full Schema (Mermaid)

```mermaid
erDiagram

    %% ── CORE ENTITIES ─────────────────────────────────────────────

    users {
        uuid        id           PK
        varchar     email        UK
        varchar     password
        enum        role         "admin_system|admin_school|teacher|parent|student"
        uuid        school_id    FK "NULL nếu admin_system"
        timestamp   created_at
        timestamp   updated_at
    }

    schools {
        uuid        id           PK
        varchar     name
        varchar     school_code  UK "tối đa 8 ký tự"
        timestamp   created_at
        timestamp   updated_at
    }

    academic_years {
        uuid        id           PK
        uuid        school_id    FK
        varchar     name         "VD: 2025-2026"
        date        start_date
        date        end_date
        enum        status       "preparing|in_progress|completed"
        timestamp   created_at
        timestamp   updated_at
    }

    subjects {
        uuid        id               PK
        uuid        academic_year_id FK
        varchar     name
        timestamp   created_at
        timestamp   updated_at
    }

    %% ── PEOPLE ────────────────────────────────────────────────────

    teachers {
        uuid        id            PK
        uuid        user_id       FK-UK "1:1 với users"
        uuid        school_id     FK
        varchar     full_name
        date        date_of_birth
        varchar     address
        varchar     citizen_id    UK "CCCD"
        varchar     teacher_code  UK
        enum        gender        "male|female|other"
        enum        work_status   "active|resigned"
        timestamp   created_at
        timestamp   updated_at
    }

    parents {
        uuid        id                   PK
        uuid        user_id              FK-UK "1:1 với users"
        varchar     father_name
        varchar     father_occupation
        date        father_date_of_birth
        varchar     mother_name
        varchar     mother_occupation
        date        mother_date_of_birth
        varchar     phone
        timestamp   created_at
        timestamp   updated_at
    }

    students {
        uuid        id            PK
        varchar     student_code  UK "[school_code][year][seq]"
        uuid        user_id       FK-UK "1:1 với users"
        uuid        school_id     FK
        varchar     full_name
        date        date_of_birth
        varchar     address
        enum        gender        "male|female|other"
        uuid        parent_id     FK "N HS → 1 PH"
        enum        study_status  "studying|dropped_out"
        timestamp   created_at
        timestamp   updated_at
    }

    %% ── CLASS MANAGEMENT ──────────────────────────────────────────

    classes {
        uuid        id                  PK
        uuid        academic_year_id    FK
        varchar     name                "VD: 10A1"
        uuid        homeroom_teacher_id FK "GVCN"
        timestamp   created_at
        timestamp   updated_at
    }

    class_students {
        uuid        id               PK
        uuid        class_id         FK
        uuid        student_id       FK
        uuid        academic_year_id FK "denorm: BR-008-01"
        timestamp   created_at
    }

    subject_assignments {
        uuid        id         PK
        uuid        class_id   FK
        uuid        subject_id FK
        uuid        teacher_id FK "GVBM"
        timestamp   created_at
        timestamp   updated_at
    }

    %% ── SCORES ────────────────────────────────────────────────────

    score_columns {
        uuid        id                    PK
        uuid        subject_assignment_id FK
        int         coefficient           "hệ số > 0"
        int         order                 "thứ tự hiển thị"
        varchar     label                 "NULL = chưa đặt tên"
        timestamp   created_at
    }

    scores {
        uuid        id              PK
        uuid        score_column_id FK
        uuid        student_id      FK
        decimal     value           "NULL=chưa nhập; 0-10; (5,2)"
        timestamp   created_at
        timestamp   updated_at
    }

    %% ── COMMUNICATION ─────────────────────────────────────────────

    comments {
        uuid        id           PK
        uuid        teacher_id   FK "GVCN"
        uuid        student_id   FK
        uuid        class_id     FK
        text        content
        timestamp   scheduled_at "thời gian dự kiến gửi"
        enum        status       "scheduled|sent"
        timestamp   created_at
        timestamp   updated_at
    }

    notifications {
        uuid        id            PK
        uuid        user_id       FK "người nhận (PH hoặc HS)"
        uuid        comment_id    FK
        varchar     email_to
        varchar     email_subject
        text        email_body
        boolean     is_sent       "email đã gửi chưa"
        boolean     is_read       "người nhận đã đọc chưa"
        timestamp   sent_at       "NULL nếu chưa gửi"
        timestamp   created_at
    }

    %% ── UTILITY ───────────────────────────────────────────────────

    password_reset_tokens {
        uuid        id         PK
        uuid        user_id    FK
        varchar     token      UK
        timestamp   expires_at "created_at + 15 phút"
        timestamp   used_at    "NULL = chưa dùng"
        timestamp   created_at
    }

    student_code_sequences {
        uuid        id        PK
        uuid        school_id
        int         year
        int         last_seq  "default 0"
    }

    %% ── RELATIONSHIPS ─────────────────────────────────────────────

    users                ||--o|  teachers             : "1 user → 0..1 teacher"
    users                ||--o|  students             : "1 user → 0..1 student"
    users                ||--o|  parents              : "1 user → 0..1 parent"
    users                }o--||  schools              : "thuộc trường (NULL nếu admin_system)"
    users                ||--o{  notifications        : "nhận thông báo"
    users                ||--o{  password_reset_tokens: "yêu cầu reset PW"

    schools              ||--|{  academic_years       : "1 trường → N năm học"
    schools              ||--|{  teachers             : "1 trường → N giáo viên"
    schools              ||--|{  students             : "1 trường → N học sinh"

    academic_years       ||--|{  subjects             : "1 năm học → N môn học"
    academic_years       ||--|{  classes              : "1 năm học → N lớp"
    academic_years       ||--o{  class_students       : "denorm FK (BR-008-01)"

    teachers             ||--o{  classes              : "GVCN: 0..1 lớp / năm học"
    teachers             ||--o{  subject_assignments  : "GVBM: dạy N lớp-môn"
    teachers             ||--o{  comments             : "tạo nhận xét"

    parents              ||--|{  students             : "1 PH → N học sinh (anh chị em)"

    students             ||--o{  class_students       : "1 HS → 0..1 lớp / năm học"
    students             ||--o{  scores               : "có điểm"
    students             ||--o{  comments             : "được nhận xét"

    classes              ||--|{  class_students       : "1 lớp → N học sinh"
    classes              ||--|{  subject_assignments  : "1 lớp → N phân công BM"
    classes              ||--o{  comments             : "context lớp"

    subject_assignments  ||--o{  score_columns        : "1 phân công → N cột điểm"

    score_columns        ||--o{  scores               : "1 cột điểm → N điểm HS"

    comments             ||--o{  notifications        : "1 nhận xét → N thông báo"
```

---

## Bảng quan hệ tóm tắt

| Bảng | Quan hệ với | Loại | Ghi chú |
|------|------------|------|---------|
| `users` | `schools` | N:1 | NULL nếu role = admin_system |
| `users` | `teachers` | 1:1 | qua user_id |
| `users` | `students` | 1:1 | qua user_id |
| `users` | `parents` | 1:1 | qua user_id |
| `schools` | `academic_years` | 1:N | |
| `schools` | `teachers` | 1:N | |
| `schools` | `students` | 1:N | |
| `academic_years` | `subjects` | 1:N | môn học theo từng năm |
| `academic_years` | `classes` | 1:N | |
| `teachers` | `classes` | 1:N | homeroom_teacher_id (GVCN) |
| `teachers` | `subject_assignments` | 1:N | GVBM |
| `parents` | `students` | 1:N | anh/chị/em cùng phụ huynh |
| `classes` | `class_students` | 1:N | junction table |
| `students` | `class_students` | 1:N | 1 HS / 1 lớp / năm học (unique) |
| `classes` | `subject_assignments` | 1:N | |
| `subjects` | `subject_assignments` | 1:N | |
| `subject_assignments` | `score_columns` | 1:N | cột điểm do GVBM tạo |
| `score_columns` | `scores` | 1:N | |
| `students` | `scores` | 1:N | |
| `comments` | `notifications` | 1:N | 1 nhận xét → nhiều người nhận |

---

## Unique Constraints quan trọng

| Constraint | Bảng | Ý nghĩa |
|-----------|------|---------|
| `(academic_year_id, name)` | `classes` | Không trùng tên lớp / năm học |
| `(academic_year_id, homeroom_teacher_id)` | `classes` | 1 GV chỉ chủ nhiệm 1 lớp / năm học |
| `(class_id, student_id)` | `class_students` | Không thêm trùng HS vào lớp |
| `(academic_year_id, student_id)` | `class_students` | **BR-008-01**: 1 HS / 1 lớp / năm học |
| `(score_column_id, student_id)` | `scores` | 1 HS chỉ có 1 điểm mỗi cột |
| `(school_id, year)` | `student_code_sequences` | Sequence theo trường + năm |
| Partial unique index | `academic_years` | 1 trường chỉ có 1 năm học `in_progress` |

---

## Enum Types

```
Role               → admin_system | admin_school | teacher | parent | student
AcademicYearStatus → preparing | in_progress | completed
Gender             → male | female | other
WorkStatus         → active | resigned
StudyStatus        → studying | dropped_out
CommentStatus      → scheduled | sent
```

---

## Công thức nghiệp vụ

```
ĐTB (FR-015) = SUM(value × coefficient) / SUM(coefficient)
             -- Chỉ tính các cột có value IS NOT NULL
             -- Nếu không có điểm nào → trả về NULL
```
