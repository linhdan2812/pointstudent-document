# Database Design — Overview (Quan hệ giữa các bảng)

> Chú thích ký hiệu:
> - `||——||` = 1 : 1
> - `||——|{` = 1 : N (bắt buộc)
> - `||——o{` = 1 : 0..N (có thể không có)
> - `||——o|` = 1 : 0..1 (có thể không có)
> - `}o——||` = N : 1 (optional bên N)

---

## Sơ đồ tổng thể

```mermaid
erDiagram

    %% ════════════════════════════════════════
    %% NHÓM 1 — TỔ CHỨC (School / Year / Subject)
    %% ════════════════════════════════════════

    schools ||--|{ academic_years     : "1 trường  →  N năm học"
    schools ||--|{ teachers           : "1 trường  →  N giáo viên"
    schools ||--|{ students           : "1 trường  →  N học sinh"

    academic_years ||--|{ subjects    : "1 năm học  →  N môn học"
    academic_years ||--|{ classes     : "1 năm học  →  N lớp"

    %% ════════════════════════════════════════
    %% NHÓM 2 — TÀI KHOẢN (Users)
    %% ════════════════════════════════════════

    users }o--o| schools              : "thuộc trường (NULL nếu admin_system)"
    users ||--o| teachers             : "1 : 1"
    users ||--o| students             : "1 : 1"
    users ||--o| parents              : "1 : 1"

    %% ════════════════════════════════════════
    %% NHÓM 3 — LỚP HỌC
    %% ════════════════════════════════════════

    teachers ||--o{ classes           : "GVCN  —  0..1 lớp / năm"
    parents  ||--|{ students          : "1 PH  →  N con (anh chị em)"

    classes  ||--|{ class_students    : "1 lớp  →  N học sinh"
    students ||--o{ class_students    : "1 HS  →  1 lớp / năm học"
    academic_years ||--o{ class_students : "denorm (BR-008-01)"

    %% ════════════════════════════════════════
    %% NHÓM 4 — PHÂN CÔNG BỘ MÔN
    %% ════════════════════════════════════════

    classes   ||--|{ subject_assignments : "1 lớp  →  N phân công"
    subjects  ||--|{ subject_assignments : "1 môn  →  N phân công"
    teachers  ||--o{ subject_assignments : "GVBM  →  N lớp-môn"

    %% ════════════════════════════════════════
    %% NHÓM 5 — ĐIỂM SỐ
    %% ════════════════════════════════════════

    subject_assignments ||--o{ score_columns : "1 phân công  →  N cột điểm"
    score_columns       ||--o{ scores        : "1 cột điểm  →  N điểm HS"
    students            ||--o{ scores        : "1 HS  →  N điểm"

    %% ════════════════════════════════════════
    %% NHÓM 6 — NHẬN XÉT & THÔNG BÁO
    %% ════════════════════════════════════════

    teachers  ||--o{ comments      : "GVCN tạo nhận xét"
    students  ||--o{ comments      : "được nhận xét"
    classes   ||--o{ comments      : "context lớp"

    comments  ||--o{ notifications : "1 nhận xét  →  N thông báo"
    users     ||--o{ notifications : "người nhận (PH / HS)"

    %% ════════════════════════════════════════
    %% NHÓM 7 — TIỆN ÍCH
    %% ════════════════════════════════════════

    users ||--o{ password_reset_tokens : "yêu cầu reset mật khẩu"
```

---

## Luồng dữ liệu theo nghiệp vụ

```mermaid
flowchart TD
    subgraph ORG["🏫 Tổ chức"]
        SCH[schools]
        AY[academic_years]
        SUB[subjects]
    end

    subgraph PEOPLE["👤 Con người"]
        USR[users]
        TCH[teachers]
        STU[students]
        PAR[parents]
    end

    subgraph CLASS["📚 Lớp học"]
        CLS[classes]
        CS[class_students]
        SA[subject_assignments]
    end

    subgraph SCORE["📊 Điểm số"]
        SC[score_columns]
        SCR[scores]
    end

    subgraph COMM["💬 Nhận xét"]
        CMT[comments]
        NTF[notifications]
    end

    %% Org links
    SCH -->|"1→N"| AY
    AY  -->|"1→N"| SUB
    AY  -->|"1→N"| CLS

    %% People links
    SCH -->|"1→N"| TCH
    SCH -->|"1→N"| STU
    USR -.-|"1:1"| TCH
    USR -.-|"1:1"| STU
    USR -.-|"1:1"| PAR
    PAR -->|"1→N"| STU

    %% Class links
    TCH -->|"GVCN\n0..1 lớp/năm"| CLS
    CLS -->|"1→N"| CS
    STU -->|"1→1 lớp/năm"| CS
    CLS -->|"1→N"| SA
    SUB -->|"1→N"| SA
    TCH -->|"GVBM\n0→N"| SA

    %% Score links
    SA  -->|"1→N"| SC
    SC  -->|"1→N"| SCR
    STU -->|"1→N"| SCR

    %% Comment links
    TCH -->|"tạo"| CMT
    STU -->|"nhận"| CMT
    CLS -->|"context"| CMT
    CMT -->|"1→N"| NTF
    USR -->|"nhận"| NTF

    %% Styles
    style ORG    fill:#dbeafe,stroke:#3b82f6
    style PEOPLE fill:#dcfce7,stroke:#22c55e
    style CLASS  fill:#fef9c3,stroke:#eab308
    style SCORE  fill:#fce7f3,stroke:#ec4899
    style COMM   fill:#ede9fe,stroke:#8b5cf6
```

---

## Bảng tóm tắt quan hệ

| Từ | Đến | Quan hệ | Ghi chú |
|----|-----|---------|---------|
| `users` | `schools` | N : 0..1 | NULL nếu admin_system |
| `users` | `teachers` | **1 : 1** | |
| `users` | `students` | **1 : 1** | |
| `users` | `parents` | **1 : 1** | |
| `schools` | `academic_years` | **1 : N** | |
| `schools` | `teachers` | **1 : N** | |
| `schools` | `students` | **1 : N** | |
| `academic_years` | `subjects` | **1 : N** | môn theo năm |
| `academic_years` | `classes` | **1 : N** | |
| `parents` | `students` | **1 : N** | anh/chị/em |
| `teachers` | `classes` | 1 : 0..N | qua homeroom_teacher_id (GVCN) |
| `classes` | `class_students` | **1 : N** | junction |
| `students` | `class_students` | 1 : 0..N | 1 HS / 1 lớp / năm (unique) |
| `classes` | `subject_assignments` | **1 : N** | |
| `subjects` | `subject_assignments` | **1 : N** | |
| `teachers` | `subject_assignments` | 1 : 0..N | GVBM |
| `subject_assignments` | `score_columns` | 1 : 0..N | |
| `score_columns` | `scores` | 1 : 0..N | |
| `students` | `scores` | 1 : 0..N | |
| `teachers` | `comments` | 1 : 0..N | GVCN tạo |
| `students` | `comments` | 1 : 0..N | |
| `classes` | `comments` | 1 : 0..N | context |
| `comments` | `notifications` | 1 : 0..N | |
| `users` | `notifications` | 1 : 0..N | người nhận |
| `users` | `password_reset_tokens` | 1 : 0..N | |
