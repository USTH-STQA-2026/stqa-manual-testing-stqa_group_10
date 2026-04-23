[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/WBjeZ6BC)
# STQA Library Manual Testing — Starter Template

Bài tập thực hành **Kiểm thử thủ công (Manual Testing)** cho môn **Kiểm thử và Đảm bảo chất lượng phần mềm (STQA)**.
(*A hands-on **Manual Testing** assignment for the **Software Testing & Quality Assurance (STQA)** course.*)

Hệ thống cần kiểm thử: **Quản lý mượn sách Thư viện ABC** — [https://stqa.rbc.vn](https://stqa.rbc.vn)

> **📚 Hệ thống hư cấu / Fictional System**: Thư viện ABC là hệ thống **hư cấu** được thiết kế cho mục đích học tập. Tên nhân vật, tổ chức và dữ liệu đều là giả lập. / *ABC Library is a **fictional** system designed for educational purposes. All names, organizations, and data are simulated.*

---

## 👥 Thông tin nhóm / Team Information

> **⚠️ Sinh viên: Điền thông tin nhóm vào bảng dưới đây trước khi nộp bài.**

| | Thông tin |
|---|---|
| **Tên nhóm** | `<!-- VD: Nhóm 1 -->` |
| **Lớp** | `<!-- VD: SE001.P11 -->` |
| **Học kỳ** | `<!-- VD: HK2 2025-2026 -->` |

| # | MSSV | Họ và tên | Vai trò |
|---|------|-----------|---------|
| 1 | | | Nhóm trưởng |
| 2 | | | Thành viên |
| 3 | | | Thành viên |
| 4 | | | Thành viên |

---

## 📖 Trước khi bắt đầu — Đọc gì, ở đâu? / Before You Start

Bạn đang cầm trên tay một **hệ thống phần mềm** cần kiểm thử. Trước khi viết bất kỳ test case nào, hãy hiểu rõ bối cảnh:

### Yêu cầu đến từ đâu? (*Where do requirements come from?*)

Yêu cầu phần mềm được ghi nhận trong tài liệu **SRS (Software Requirements Specification)** — đặc tả yêu cầu phần mềm. Đây là tài liệu mà **khách hàng** (thư viện ABC) và **đội phát triển** đã thống nhất.

👉 Đọc: [docs/SRS-library-system.md](docs/SRS-library-system.md) — 8 yêu cầu chức năng (REQ-01 → REQ-08)
👉 Tham khảo: [docs/BRD-yeu-cau-nghiep-vu.md](docs/BRD-yeu-cau-nghiep-vu.md) — Yêu cầu nghiệp vụ ban đầu từ khách hàng (BRD → SRS)

### Những ai liên quan? (*Who are the stakeholders?*)

| Vai trò | Ai? | Làm gì? |
|---------|-----|---------|
| **Khách hàng** (*Customer*) | Thư viện ABC | Đưa ra yêu cầu nghiệp vụ ([BRD](docs/BRD-yeu-cau-nghiep-vu.md)) → BA viết [SRS](docs/SRS-library-system.md) |
| **Đội phát triển** (*Dev Team*) | Nhóm lập trình | Xây dựng hệ thống theo SRS |
| **Tester / QC** (*Quality Control*) | **Bạn — sinh viên** | Kiểm thử hệ thống, tìm lỗi, báo cáo |
| **QA Lead** | Giảng viên / Hướng dẫn | Review kết quả, đánh giá chất lượng |
| **Người dùng cuối** (*End User*) | Thủ thư + Thành viên thư viện | Sử dụng hệ thống hàng ngày |

### Tester dựa vào đâu? (*What do you base your testing on?*)

| Bạn dựa vào... | Để làm gì... |
|----------------|-------------|
| **SRS** (đặc tả yêu cầu) | Biết hệ thống **phải** làm gì → viết **kết quả mong đợi** (expected result) |
| **Dữ liệu kiểm thử** (test accounts, seed data) | Chuẩn bị **dữ liệu đầu vào** cho từng test case |
| **Kỹ thuật thiết kế** (EP, BVA, Decision Table) | Quyết định **test bao nhiêu, test những giá trị nào** |
| **Giao diện hệ thống** (UI thực tế) | Quan sát **kết quả thực tế** (actual result) khi chạy test |

### Software Testing vs Quality Assurance — Khác nhau thế nào?

| | **Software Testing** (Kiểm thử) | **Quality Assurance** (Đảm bảo chất lượng) |
|--|---|---|
| **Mục đích** | Tìm lỗi (*defects*) trong sản phẩm | Ngăn ngừa lỗi từ quy trình |
| **Câu hỏi** | "Phần mềm có lỗi không?" | "Quy trình có giúp tạo phần mềm tốt không?" |
| **Hoạt động** | Chạy test case, ghi kết quả, báo cáo lỗi | Thiết lập quy trình, review, audit, cải tiến |
| **Thời điểm** | Sau khi có sản phẩm (reactive) | Từ đầu dự án (proactive) |
| **Vai trò** | Tester, QC (Quality Control) | QA Engineer, QA Lead |
| **Trong bài này** | ✅ **Bạn đang làm Testing** — viết TC, chạy test, báo cáo bug | ✅ Bạn cũng thực hành QA — viết tổng hợp, đề xuất cải tiến |

> 💡 **Tóm lại**: Bài tập này chủ yếu là **Software Testing** (tìm lỗi), nhưng phần Summary (báo cáo tổng hợp) là hoạt động **Quality Assurance** (đánh giá chất lượng, đề xuất cải tiến).

---

## 📁 Cấu trúc dự án / Project Structure

```
stqa-library-manual-starter/
├── README.md                    # File này — hướng dẫn bắt đầu
├── docs/
│   ├── ASSIGNMENT.md            # 📋 Đề bài + rubric chấm điểm
│   ├── SRS-library-system.md    # 📄 Đặc tả yêu cầu phần mềm (8 REQ)
│   ├── test-accounts.md         # 👤 Tài khoản kiểm thử
│   ├── glossary.md              # 📖 Thuật ngữ kiểm thử
│   ├── testing-scope.md         # 🎯 Phạm vi kiểm thử
│   └── faq.md                   # ❓ Câu hỏi thường gặp
├── examples/
│   ├── sample-test-case.md      # ✅ Ví dụ 1 test case hoàn chỉnh
│   └── sample-bug-report.md     # 🐛 Ví dụ 1 bug report hoàn chỉnh
└── submissions/                 # 📝 Bạn làm bài ở đây
    ├── test-cases.md            # Template bảng trường hợp kiểm thử
    ├── test-execution.md        # Template kết quả thực thi
    ├── bug-reports.md           # Template báo cáo lỗi
    └── summary.md               # Template báo cáo tổng hợp
```

---

## 🚀 Bắt đầu nhanh / Quick Start

### Bước 1: Đọc đề bài
👉 [docs/ASSIGNMENT.md](docs/ASSIGNMENT.md) — Yêu cầu bài tập, tiêu chí chấm điểm

### Bước 2: Hiểu hệ thống
👉 [docs/SRS-library-system.md](docs/SRS-library-system.md) — Hệ thống **phải** làm gì (8 yêu cầu)
👉 [docs/test-accounts.md](docs/test-accounts.md) — Tài khoản để đăng nhập kiểm thử

### Bước 3: Xem ví dụ mẫu
👉 [examples/sample-test-case.md](examples/sample-test-case.md) — TC mẫu hoàn chỉnh
👉 [examples/sample-bug-report.md](examples/sample-bug-report.md) — Bug report mẫu

### Bước 4: Viết bài trong `submissions/`
👉 [submissions/test-cases.md](submissions/test-cases.md) — Viết test case từ TC-01
👉 Chạy test trên hệ thống → ghi kết quả vào `test-execution.md`
👉 Mỗi TC Fail → tạo bug report trong `bug-reports.md`
👉 Tổng hợp trong `summary.md`

### Bước 5: Nộp bài

```bash
# Fork repo → Clone về máy
git clone https://github.com/<your-team>/stqa-library-manual-<team-name>.git
cd stqa-library-manual-<team-name>

# Viết bài trong submissions/
# Commit và push
git add .
git commit -m "test: complete A1 manual testing submission"
git push origin main
```

---

## ⚙️ Truy cập hệ thống kiểm thử

- **URL**: [https://stqa.rbc.vn](https://stqa.rbc.vn)
- **Trình duyệt**: Chrome bản mới nhất
- **Ngôn ngữ**: Tiếng Việt (mặc định)
- **Reset dữ liệu**: Refresh trang HOẶC Thủ thư nhấn nút 🔄

> ⚠️ Dữ liệu lưu trong bộ nhớ trình duyệt. Refresh trang = dữ liệu trở về trạng thái ban đầu.

---

## 📚 Tài liệu tham khảo nhanh

| Bạn muốn... | Đi đến |
|-------------|--------|
| Xem đề bài + rubric | [docs/ASSIGNMENT.md](docs/ASSIGNMENT.md) |
| Xem yêu cầu nghiệp vụ (BRD) | [docs/BRD-yeu-cau-nghiep-vu.md](docs/BRD-yeu-cau-nghiep-vu.md) |
| Đọc yêu cầu hệ thống (SRS) | [docs/SRS-library-system.md](docs/SRS-library-system.md) |
| Xem tài khoản test | [docs/test-accounts.md](docs/test-accounts.md) |
| Tra thuật ngữ | [docs/glossary.md](docs/glossary.md) |
| Xem phạm vi test | [docs/testing-scope.md](docs/testing-scope.md) |
| **Liên kết textbook ↔ bài tập** | [docs/textbook-concepts.md](docs/textbook-concepts.md) |
| **Bài tập nhóm thảo luận** | [docs/group-exercises.md](docs/group-exercises.md) |
| Xem ví dụ TC mẫu | [examples/sample-test-case.md](examples/sample-test-case.md) |
| Xem ví dụ bug report | [examples/sample-bug-report.md](examples/sample-bug-report.md) |
| Làm bài | [submissions/](submissions/) |
| Câu hỏi thường gặp | [docs/faq.md](docs/faq.md) |

---

## ⚠️ Quy định quan trọng / Important Rules

- **Không commit dữ liệu nhạy cảm** (token, mật khẩu cá nhân).
- **Không sao chép bài nhóm khác** — mỗi nhóm phải có test case và bug report riêng.
- **Mỗi thành viên nên commit ít nhất 1 lần** để thể hiện đóng góp.
- **Tuân thủ format template** trong `submissions/`.
