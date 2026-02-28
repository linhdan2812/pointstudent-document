# PM DELIVERY PLAN – HỆ THỐNG QUẢN LÝ ĐIỂM HỌC SINH

## 1. Thông tin chung
- Start date: 02/03
- Mô hình: Agile – Scrum
- Số sprint: 3 (2 tuần/sprint)
- Căn cứ: SRS (FR-001 → FR-018)

---

## 2. Nguồn lực dự án

| Role | Số lượng | Giờ/ngày |
|---|---:|---:|
| PM | 1 | — |
| Dev (Fresher) | 1 | 8h |
| Tester (Fresher) | 1 | 4h |

Flow: Dev → Test → Fix Bug → Verify Bug

---

## 3. Module hệ thống

| Module | Tên |
|---|---|
| M1 | Authentication |
| M2 | School Master Data |
| M3 | User Management |
| M4 | Class Management |
| M5 | Teacher View |
| M6 | Score Management |
| M7 | Portal & Comment |

---

## 4. Mapping FR theo SRS

| FR | Tên | Module |
|---|---|---|
| FR-001 | Đăng nhập | M1 |
| FR-018 | Quên mật khẩu | M1 |
| FR-002 | QL trường học | M2 |
| FR-003 | QL năm học | M2 |
| FR-004 | QL môn học | M2 |
| FR-005 | QL giáo viên | M3 |
| FR-006 | QL học sinh | M3 |
| FR-007 | QL lớp học | M4 |
| FR-008 | DS HS lớp | M4 |
| FR-009 | Phân công GVBM | M4 |
| FR-010 | Dashboard GV | M5 |
| FR-011 | CN – bảng điểm | M5 |
| FR-012 | CN – info HS | M5 |
| FR-014 | BM – quản lý điểm | M6 |
| FR-015 | Tính ĐTB | M6 |
| FR-013 | Nhận xét HS | M7 |
| FR-016 | Cổng PH | M7 |
| FR-017 | Cổng HS | M7 |

---

## 5. Master Estimation (theo giờ)

### Sprint 1

| FR | Dev | Test | Fix | Verify | Total |
|---|---:|---:|---:|---:|---:|
| FR-001 | 8 | 4 | 2 | 1 | 15 |
| FR-018 | 6 | 4 | 2 | 1 | 13 |
| FR-002 | 10 | 6 | 3 | 2 | 21 |
| FR-003 | 8 | 4 | 2 | 1 | 15 |
| FR-004 | 6 | 4 | 2 | 1 | 13 |
| FR-005 | 12 | 6 | 4 | 2 | 24 |

### Sprint 2

| FR | Dev | Test | Fix | Verify | Total |
|---|---:|---:|---:|---:|---:|
| FR-006 | 16 | 8 | 5 | 3 | 32 |
| FR-007 | 10 | 6 | 3 | 2 | 21 |
| FR-008 | 8 | 4 | 2 | 1 | 15 |
| FR-009 | 8 | 4 | 2 | 1 | 15 |
| FR-010 | 6 | 4 | 2 | 1 | 13 |

### Sprint 3

| FR | Dev | Test | Fix | Verify | Total |
|---|---:|---:|---:|---:|---:|
| FR-011 | 6 | 4 | 2 | 1 | 13 |
| FR-012 | 4 | 2 | 1 | 1 | 8 |
| FR-014 | 16 | 8 | 5 | 3 | 32 |
| FR-015 | 6 | 4 | 2 | 1 | 13 |
| FR-013 | 12 | 6 | 4 | 2 | 24 |
| FR-016 | 8 | 4 | 3 | 2 | 17 |
| FR-017 | 6 | 4 | 2 | 1 | 13 |

---

## 6. Tổng effort

- Dev: 156h
- Test: 86h
- Fix Bug: 48h
- Verify Bug: 27h
- **Overall: 317h**


---

## 7. Bảng viết tắt (Glossary)

| Từ viết tắt | Ý nghĩa |
|---|---|
| PM | Project Manager – Quản lý dự án |
| BA | Business Analyst – Phân tích nghiệp vụ |
| FR | Functional Requirement – Yêu cầu chức năng |
| SRS | Software Requirements Specification |
| ET | Estimation – Ước lượng effort |
| CRUD | Create – Read – Update – Delete |
| HS | Học sinh |
| PH | Phụ huynh |
| GV | Giáo viên |
| GVCN | Giáo viên chủ nhiệm |
| GVBM | Giáo viên bộ môn |
| CN | Chủ nhiệm |
| BM | Bộ môn |
| ĐTB | Điểm trung bình |
| UI | User Interface – Giao diện người dùng |
| API | Application Programming Interface |
| DoD | Definition of Done – Điều kiện hoàn thành |
| Sprint | Chu kỳ phát triển trong Agile |

