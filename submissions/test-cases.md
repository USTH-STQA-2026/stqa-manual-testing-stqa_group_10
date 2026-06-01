# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | `<!-- Tên nhóm -->` |
| **Ngày tạo** | `<!-- DD/MM/YYYY -->` |
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

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| | | | | | | | |

---
<!-- Phía trên là bảng mẫu, thành viên không được viết thẳng vào đó, mà phải copy và paste lại đúng vị trí REQ mà mình được giao -->

### REQ-01
``
<!-- Dưới đây là vị trí để viết bảng. Lưu ý: Bảng phải đúng theo như format bên trên, nếu muốn xuống dòng, sử dụng tag <br> giữa vị trí cần enter -->

### REQ-02


### REQ-03


### REQ-04


### REQ-05


### REQ-06
| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-18 | Kiểm tra sách quá hạn| Phiếu mượn có `dueDate` ≤ ngày hiện tại | 1.Đăng nhập bằng tài khoản thủ thư <br><br>2.Chọn tab 'Mượn trả'<br><br>3.Bấm nút 'Kiểm tra sách quá hạn' | Tài khoản thủ thư | 1.Thông báo 'đã cập nhật: XX' sách quá hạn <br><br>2.Trạng thái sách chuyển từ 'đang mượn' thành 'quá hạn' | REQ-06 | |
| TC-19 | Thành viên thấy phiếu của mình nếu quá hạn | Phiếu mượn có `dueDate` ≤ ngày hiện tại | 1.Đăng nhập bằng tài khoản thành viên <br><br>2.Chọn tab 'Mượn trả' | Tài khoản thành viên | Trạng thái sách đang mượn chuyển từ 'đang mượn' thành 'quá hạn'| REQ-06 | |

### REQ-07


### REQ-08 - Tra cứu phiếu mượn
| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-29 | Thủ thư xem tất cả phiếu mượn của mọi thành viên | Thành viên mượn sách từ thư viện | 1.Đăng nhập bằng tài khoản thủ thư<br><br>2.Chọn tab 'mượn trả' | Tài khoản thủ thư | Tab 'tất cả phiếu mượn' sẽ hiện ra danh sách phiếu mượn với tên sách, tên người mượn, ngày mượn, hạn trả, và trạng thái mượn của sách. | REQ-08 | |
| TC-30 | Thành viên xem tất cả các phiếu mượn sách của mình | Thành viên mượn sách từ thư viện | 1.Đăng nhập bằng tài khoản thành viên<br><br>2. Chọn tab 'mượn trả' | Tài khoản thành viên | Tab 'phiếu mượn của tôi' sẽ hiện tất cả những phiếu sách mà chỉ có sách mà thành viên đang đăng nhập mượn có với tên sách, mã phiếu, tên thành viên, ngày mượn và hạn trả, và trạng thái sách. | REQ-08 | |


## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
