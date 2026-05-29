# Test Cases — Bảng trường hợp kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 10 |
| **Ngày tạo** | 26/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

*(Phần này do Người 1 tổng hợp dựa trên phân tích của cả nhóm - Sẽ điền sau khi chốt yêu cầu)*

---

## Bước 2: Test Cases chi tiết

###  Tester 2 — REQ 01 và REQ 07 

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-01 | Kiểm tra đăng nhập thành công với tài khoản hợp lệ đã được đăng kí | 1. Người dùng chưa đăng nhập <br><br> 2. Tài khoản đã tồn tại và đã kích hoạt trên hệ thống | 1. Truy cập trang đăng nhập <br><br> 2. Nhập email hợp lệ <br><br> 3. Nhập mật khẩu đúng <br><br> 4. Ấn đăng nhập | 1. Điền email : librarian@library.com <br><br> 2. Nhập mật khẩu : admin123 | 1. Chuyển sang trang chủ thành công <br><br> 2. App bar hiển thị đúng tên người dùng và vai trò : Nguyễn Thủ Thư(Thủ thư) | REQ-01 | EP |
| TC-02 | Kiểm tra báo lỗi khi để trống cả email và mật khẩu | Người dùng chưa đăng nhập | 1. Truy cập trang web phần đăng nhập tài khoản <br><br> 2. Không điền gì vào phần email và mật khẩu <br><br> 3. Ấn đăng nhập | Để trống cả phần email và cả mật khẩu | Hệ thống hiển thị thông báo lỗi : Vui lòng nhập email và mật khẩu | REQ-01 | Decision Table | 
| TC-03 | Kiểm tra báo lỗi khi nhập đúng email nhưng để trống mật khẩu | Người dùng chưa đăng nhập | 1. Truy cập vào web chỗ đăng nhập tài khoản <br><br> 2. Nhập đúng email  <br><br> 3. Để trống mật khẩu <br><br> 4. Ấn đăng nhập | 1. Nhập email : librarian@library.com <br><br> 2. Mật khẩu : (để trống) <br><br> | Hệ thống ngăn chặn đăng nhập và hiển thị thông báo lỗi yêu cầu nhập mật khẩu (hoặc dùng chung thông báo bỏ trống). | REQ-01 | BVA |
| TC-04 | Kiểm tra báo lỗi khi nhập đúng email nhưng sai mật khẩu | 1. Người dùng chưa đăng nhập <br><br> 2. Tài khoản đã tồn tại trên hệ thống | 1. Truy cập trang web phần đăng nhập <br><br> 2. Nhập email hợp lệ <br><br> 3. Điền sai mật khẩu <br><br> 4. Ấn đăng nhập | 1. Nhập email : librarian@library.com <br><br> 2. Nhập mật khẩu sai : hehe | Hệ thống hiển thị thông báo lỗi : Mật khẩu không đúng | REQ-01 | EP | 
| TC-05 | Kiểm tra đăng nhập khi cố tình viết hoa chữ cái đầu tiên của email | 1. Người dùng chưa đăng nhập <br><br> 2. Hệ thống đã có tài khoản librarian@library.com | 1. Vào trang đăng nhập <br><br> 2. Nhập email viết hoa chữ cái đầu tiên <br><br> 3. Nhập mật khẩu đúng <br><br> 4. Ấn đăng nhập | 1. Nhập email : Librarian@library.com <br><br> 2. Nhập mật khẩu : admin123 | Hệ thống tự động chuyển đổi mail về chữ thường, đăng nhập thành công và chuyển hướng sang trang chủ | REQ-01 | EP |
| TC-06 | Kiểm tra tính bảo mật của ô nhập mật khẩu | Người dùng đang ở trang đăng nhập | Nhập kí tự vào ô mật khẩu | Nhập mật khẩu : admin123 | Các ký tự nhập vào phải được ẩn đi dưới dạng dấu chấm (●●●●●) hoặc dấu sao (*****) | REQ-01 | Black-box Testing |
| TC-07 | Kiểm tra báo lỗi khi email nhập sai định dạng cấu trúc | Người dùng chưa đăng nhập | 1. Truy cập trang đăng nhập <br><br> 2. Nhập email sai định dạng cấu trúc <br><br> 3. Nhập mật khẩu bất kì <br><br> 4. Ấn đăng nhập | 1. Nhập email : ngochanhiepdepzai <br><br> 2. Điền mật khẩu bất kì | Hệ thống báo lỗi định dạng email không hợp lệ và không gửi request lên server | REQ-01 | EP |
| TC-08 | Kiểm tra thêm thành viên thành công với quyền Thủ thư | Đăng nhập tài khoản có quyền Thủ thư | 1. Điền đầy đủ thông tin hợp lệ <br><br> 2. Nhấn nút : Thêm thành viên | 1. Họ và tên : Ngô Chấn Hiệp <br><br> 2. email : ngohiep010605@gmail.com <br><br> 3. Số điện thoại : 0941898905 | Thành viên mới được tạo thành công và lưu vào hệ thống,hiển thị thông báo : Thêm thành viên thành công! Mã: MEMxxx | REQ-07 | |
| TC-09 | Kiểm tra chặn quyền khi không phải Thủ thư | Đăng nhập tài khoản không có quyền Thủ thư ( ví dụ : Thành Viên ) | 1. Đăng nhập bằng tài khoản có vai trò không phải thủ thư <br><br> 2. Ấn đăng nhập | 1. email : dam.tran@email.com <br><br> 2. mật khẩu : password123 | 1. Chuyển sang trang nhập nhập <br><br> 2. Appbar hiển thị đầy đủ thông tin Trần Dựa Dẫm ( Thành Viên ) <br><br> 3. Không có nút thêm thành viên | REQ-07 | |
| TC-10 | Kiểm tra để trống tất cả các dữ liệu bắt buộc | Đăng nhập tài khoản có quyền Thủ thư | 1. Để trống tất cả các thông tin Họ và tên, email, số điện thoại <br><br> 2. Ấn thêm thành viên | Để trống dữ liệu | Hệ thống ngăn chặn việc gửi dữ liệu và hiển thị lỗi nhắc nhở nhập các trường bắt buộc | REQ-7 | |
| TC-11 | Thêm thành viên với định dạng email không hợp lệ (Có @ nhưng thiếu dấu .) | Đăng nhập tài khoản có quyền Thủ thư | 1. Nhập thông tin với email thiếu dấu chấm ở phần domain <br><br> 2. Ấn thêm thành viên | 1. Họ và tên : ngo chán hiep <br><br> 2. email : ghiep342@gmail.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống báo lỗi email không hợp lệ, không cho phép đăng ký | REQ-07 | |
| TC-12 | Thêm thành viên với định dạng email không hợp lệ (thiếu @ nhưng có dấu .) | Đăng nhập tài khoản có quyền Thủ thư | 1. Nhập thông tin với email có dấu chấm ở đầu hoặc cuối domain <br><br> 2. Ấn thêm thành viên | 1. Họ và tên : toi ten la tao <br><br> 2. email : ghiep242.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống báo lỗi email không hợp lệ, không cho phép đăng ký | REQ-07 | | 
| TC-13 | Kiểm tra tạo thành viên với Email đã tồn tại | Đăng nhập tài khoản có quyền Thủ thư | 1. Đăng nhập tài khoản có quyền Thủ thư <br><br> 2. Dùng email đã có sẵn trên hệ thống (dam.tran@email.com) | 1. Họ và tên : Ngô Chấn Hiệp <br><br> 2. email : dam.tran@email.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống ngăn chặn và hiển thị thông báo lỗi trùng lặp (ví dụ: "Email này đã tồn tại") | REQ-07 | |
---

###  Tester 3 — Phụ trách User Module & Xác thực

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|


---

###  Tester 4 — Phụ trách Logic & Nghiệp vụ nâng cao

**Bảng Decision Table (REQ-05):**
| Điều kiện / Hành động | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|-----------------------|--------|--------|--------|--------|
| **Tài khoản hoạt động?** | Có | Có | Không | Không |
| **Sách có sẵn?** | Có | Không | Có | Không |
| **Cho phép mượn?** | **YES** | **NO** | **NO** | **NO** |

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|

---

### Tester 5 - QA Analyst & Báo cáo tổng hợp

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật áp dụng |
|----------------|-------|---------|----------------------|
| Người 2 | 6 | REQ-03, REQ-04 | BVA |
| Người 3 | 6 | REQ-01, REQ-07 | EP |
| Người 4 | 6 | REQ-02, REQ-05 | Decision Table |
| Người 5 | 6 | REQ-06, REQ-08 | N/A |
| **Tổng** | **24** | **Bao phủ toàn bộ** | |# Test Cases — Bảng trường hợp kiểm thử





