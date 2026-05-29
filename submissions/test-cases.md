# Test Cases — Bảng trường hợp kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 10 |
| **Ngày tạo** | 26/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

# Test Cases — Bảng trường hợp kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 10 |
| **Ngày tạo** | 26/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |


### IDM - REQ-01: Đăng nhập (Login)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Email tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Báo lỗi "Tài khoản không tồn tại" |
| Mật khẩu đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Báo lỗi "Mật khẩu không đúng" |
| Ô nhập có rỗng? | Không rỗng | (bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Báo lỗi "Vui lòng nhập email và mật khẩu" |

---

### IDM - REQ-02: Xem danh sách sách (View Book List)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Vai trò tài khoản? | Thủ thư | `librarian@library.com` | Xem toàn bộ sách với đầy đủ chi tiết |
| | Thành viên | `ba.nguyen@email.com` | Xem toàn bộ sách với đầy đủ chi tiết |
| Trạng thái sách? | Có sẵn | BOOK001, BOOK002 | Hiển thị "Có sẵn" |
| | Đã mượn | BOOK003 | Hiển thị "Đã mượn" |
| | Thất lạc | BOOK007 | Hiển thị "Thất lạc" |
| Thông tin đầy đủ? | Đủ 5 trường | BOOK001 | Hiện Tên, Tác giả, Thể loại, Năm, Trạng thái |
| Cập nhật realtime? | Sau khi mượn | Mượn BOOK001 | Trạng thái lập tức đổi thành "Đã mượn" |
| | Sau khi trả | Trả BOOK003 | Trạng thái lập tức đổi thành "Có sẵn" |

---

### IDM - REQ-03: Tìm kiếm và lọc sách (Search & Filter)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa tồn tại? | Có (Tên sách) | `"Flutter"` | Hiện các sách chứa chữ "Flutter" |
| | Có (Tác giả) | `"Nguyễn"` | Hiện sách của tác giả "Nguyễn" |
| | Không | `"XYZ123"` | Báo "Không tìm thấy sách" |
| Phân biệt hoa/thường? | Chữ thường | `"flutter"` | Kết quả khớp với "Flutter" |
| | Chữ hoa | `"FLUTTER"` | Kết quả khớp với "Flutter" |
| Lọc theo thể loại? | Thể loại có sách | Chọn `"Công nghệ"` | Chỉ hiện sách Công nghệ |
| | Thể loại không sách| Bỏ trống/Không có | Báo "Không tìm thấy sách" |

---

### IDM - REQ-04: Mượn sách (Borrow Book)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách? | Có sẵn | BOOK001 | Cho phép mượn |
| | Đã mượn | BOOK003 | Từ chối, báo lỗi |
| | Thất lạc | BOOK007 | Từ chối, báo lỗi |
| Trạng thái thành viên? | Hoạt động | MEM002 | Cho phép mượn |
| | Tạm ngưng | MEM004 | Từ chối, báo lỗi tài khoản tạm ngưng |
| | Hết hạn | MEM005 | Từ chối, báo lỗi tài khoản hết hạn |
| Số sách đang mượn? | < 3 | MEM006 (1 cuốn) | Cho phép mượn |
| | = 3 | MEM mượn đủ 3 cuốn| Từ chối, báo lỗi vượt giới hạn |

---

### IDM - REQ-05: Trả sách (Return Book)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Phiếu thuộc về user? | Phải | MEM002 trả BOOK003 | Cho phép trả |
| | Không | MEM002 trả BOOK013 | Ẩn tùy chọn trả / Từ chối |
| Tình trạng thời gian? | Trong hạn | BR003 | Trả thành công, không cảnh báo |
| | Quá hạn | BR001 | Trả thành công **và** hiện cảnh báo quá hạn |
| | Đã trả | BR002 | Ẩn tùy chọn trả |
| Trạng thái sách sau trả?| Thành công | BOOK003 | Đổi thành "Có sẵn" |
| Trạng thái phiếu sau trả?| Thành công | BR001 | Đổi thành "Đã trả" |

---

### IDM - REQ-06: Xử lý sách quá hạn (Overdue Handling)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Người kích hoạt? | Thủ thư | `librarian@library.com` | Quét và cập nhật phiếu quá hạn |
| | Thành viên | Mọi thành viên | Ẩn nút chức năng này |
| Ngày hiện tại vs Hạn? | Trước hạn | Hôm nay < Hạn trả | Giữ nguyên trạng thái "Đang mượn" |
| | Đúng hạn | Hôm nay = Hạn trả | Đánh dấu "Quá hạn" |
| | Sau hạn | Hôm nay > Hạn trả | Đánh dấu "Quá hạn" |
| Thành viên xem quá hạn? | Có phiếu quá hạn | MEM002 | Thấy BR001 bị "Quá hạn" |
| | Không có | MEM006 | Không thấy phiếu nào quá hạn |
| Thủ thư xem quá hạn? | Sau khi quét | Thủ thư | Thấy **tất cả** phiếu quá hạn |

---

### IDM - REQ-07: Quản lý thành viên (Add New)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Vai trò thực hiện? | Thủ thư | `librarian@library.com` | Hiện form, thực hiện được |
| | Thành viên | `ba.nguyen@email.com` | Ẩn tab "Thành viên" |
| Email hợp lệ? | Hợp lệ | `new@email.com` | Chấp nhận |
| | Thiếu `.` ở domain | `new@domain` | Từ chối, báo email không hợp lệ |
| | Thiếu `@` | `newdomain.com` | Từ chối, báo email không hợp lệ |
| | Rỗng | `""` | Từ chối, báo lỗi bắt buộc nhập |
| Email duy nhất? | Chưa tồn tại | `brandnew@email.com` | Thêm thành công |
| | Đã tồn tại | `ba.nguyen@email.com` | Từ chối, báo email đã tồn tại |
| Nhập họ tên? | Đã nhập | `"Nguyễn Văn A"` | Chấp nhận |
| | Rỗng | `""` | Từ chối, báo lỗi bắt buộc nhập |
| Nhập SĐT? | Đã nhập | `"0912345678"` | Chấp nhận |
| | Rỗng | `""` | Từ chối, báo lỗi bắt buộc nhập |

---

### IDM - REQ-08: Tra cứu phiếu mượn (Borrow Receipt Lookup)

| Đặc tính | Phân vùng | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| Vai trò tra cứu? | Thủ thư | `librarian@library.com` | Xem được **toàn bộ** phiếu |
| | Thành viên | `ba.nguyen@email.com` | Chỉ xem được phiếu của MEM002 |
| Xem của người khác? | Cố ý tra cứu | MEM002 xem của MEM006| **Không được phép** |
| Thông tin phiếu đầy đủ?| Đang mượn | BR001 | Hiện ID, Sách, Ngày, "Đang mượn" |
| | Đã trả | BR002 | Hiện "Đã trả" |
| | Quá hạn | BR001 | Hiện "Quá hạn" |

## Bước 2: Test Cases

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
| TC-08 | Kiểm tra thêm thành viên thành công với quyền Thủ thư | Đăng nhập tài khoản có quyền Thủ thư | 1. Điền đầy đủ thông tin hợp lệ <br><br> 2. Nhấn nút : Thêm thành viên | 1. Họ và tên : Ngô Chấn Hiệp <br><br> 2. email : ngohiep010605@gmail.com <br><br> 3. Số điện thoại : 0941898905 | Thành viên mới được tạo thành công và lưu vào hệ thống,hiển thị thông báo : Thêm thành viên thành công! Mã: MEMxxx | REQ-07 | EP |
| TC-09 | Kiểm tra chặn quyền khi không phải Thủ thư | Đăng nhập tài khoản không có quyền Thủ thư ( ví dụ : Thành Viên ) | 1. Đăng nhập bằng tài khoản có vai trò không phải thủ thư <br><br> 2. Ấn đăng nhập | 1. email : dam.tran@email.com <br><br> 2. mật khẩu : password123 | 1. Chuyển sang trang nhập nhập <br><br> 2. Appbar hiển thị đầy đủ thông tin Trần Dựa Dẫm ( Thành Viên ) <br><br> 3. Không có nút thêm thành viên | REQ-07 | EP |
| TC-10 | Kiểm tra để trống tất cả các dữ liệu bắt buộc | Đăng nhập tài khoản có quyền Thủ thư | 1. Để trống tất cả các thông tin Họ và tên, email, số điện thoại <br><br> 2. Ấn thêm thành viên | Để trống dữ liệu | Hệ thống ngăn chặn việc gửi dữ liệu và hiển thị lỗi nhắc nhở nhập các trường bắt buộc | REQ-7 | Decision Table |
| TC-11 | Thêm thành viên với định dạng email không hợp lệ (Có @ nhưng thiếu dấu .) | Đăng nhập tài khoản có quyền Thủ thư | 1. Nhập thông tin với email thiếu dấu chấm ở phần domain <br><br> 2. Ấn thêm thành viên | 1. Họ và tên : ngo chán hiep <br><br> 2. email : ghiep342@gmail.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống báo lỗi email không hợp lệ, không cho phép đăng ký | REQ-07 | EP |
| TC-12 | Thêm thành viên với định dạng email không hợp lệ (thiếu @ nhưng có dấu .) | Đăng nhập tài khoản có quyền Thủ thư | 1. Nhập thông tin với email có dấu chấm ở đầu hoặc cuối domain <br><br> 2. Ấn thêm thành viên | 1. Họ và tên : toi ten la tao <br><br> 2. email : ghiep242.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống báo lỗi email không hợp lệ, không cho phép đăng ký | REQ-07 | EP | 
| TC-13 | Kiểm tra tạo thành viên với Email đã tồn tại | Đăng nhập tài khoản có quyền Thủ thư | 1. Đăng nhập tài khoản có quyền Thủ thư <br><br> 2. Dùng email đã có sẵn trên hệ thống (dam.tran@email.com) | 1. Họ và tên : Ngô Chấn Hiệp <br><br> 2. email : dam.tran@email.com <br><br> 3. Số điện thoại : 0941898905 | Hệ thống ngăn chặn và hiển thị thông báo lỗi trùng lặp (ví dụ: "Email này đã tồn tại") | REQ-07 | EP |
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





