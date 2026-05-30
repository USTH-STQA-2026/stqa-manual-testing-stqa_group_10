# Test Cases — Test Case Table

| Information | |
|---|---|
| **Team** | Team 10 |
| **Creation Date** | 26/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

> 📖 **Textbook:** Chapter 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Before writing Test Cases**, the team **must** analyze the input domain using the IDM table below.
> Each function needs to define: **Characteristic**, **Block/Partition**, and **Representative Value**.

### IDM - REQ-01: Login

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Email exists in DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noone@email.com` | Error: "Account does not exist" |
| Password correct? | Correct | `admin123` | Login successful |
| | Incorrect | `wrongpass` | Error: "Incorrect password" |
| Input field empty? | Not empty | (Any) | Normal processing |
| | Empty | `""` | Error: "Please enter email and password" |

---

### IDM - REQ-02: View Book List

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Account Role? | Librarian | `librarian@library.com` | View all books with full details |
| | Member | `ba.nguyen@email.com` | View all books with full details |
| Book Status? | Available | BOOK001, BOOK002 | Display "Available" |
| | Borrowed | BOOK003 | Display "Borrowed" |
| | Lost | BOOK007 | Display "Lost" |
| Information complete? | All 5 fields | BOOK001 | Show Title, Author, Category, Year, Status |
| Real-time update? | After borrowing | Borrow BOOK001 | Status immediately changes to "Borrowed" |
| | After returning | Return BOOK003 | Status immediately changes to "Available" |

---

### IDM - REQ-03: Search & Filter

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Keyword exists? | Yes (Book Title) | `"Flutter"` | Show books containing "Flutter" |
| | Yes (Author) | `"Nguyễn"` | Show books by author "Nguyễn" |
| | No | `"XYZ123"` | Display "Book not found" |
| Case-sensitive? | Lowercase | `"flutter"` | Results match "Flutter" |
| | Uppercase | `"FLUTTER"` | Results match "Flutter" |
| Filter by category? | Category with books | Select `"Technology"` | Show Technology books only |
| | Category without books| Empty/None | Display "Book not found" |

---

### IDM - REQ-04: Borrow Book

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Book Status? | Available | BOOK001 | Allow borrowing |
| | Borrowed | BOOK003 | Rejected, display error message |
| | Lost | BOOK007 | Rejected, display error message |
| Member Status? | Active | MEM002 | Allow borrowing |
| | Suspended | MEM004 | Rejected, error: account suspended |
| | Expired | MEM005 | Rejected, error: account expired |
| Number of borrowed books?| < 3 | MEM006 (1 book) | Allow borrowing |
| | = 3 | Member borrowed 3 books| Rejected, error: limit exceeded |

---

### IDM - REQ-05: Return Book

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Receipt belongs to user?| Yes | MEM002 returns BOOK003 | Allow return |
| | No | MEM002 returns BOOK013 | Hide return option / Rejected |
| Time Status? | Within due date | BR003 | Returned successfully, no warning |
| | Overdue | BR001 | Returned successfully **and** show overdue warning |
| | Already returned | BR002 | Hide return option |
| Book status after return?| Success | BOOK003 | Changes to "Available" |
| Receipt status after return?| Success | BR001 | Changes to "Returned" |

---

### IDM - REQ-06: Overdue Handling

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Triggered by? | Librarian | `librarian@library.com` | Scan and update overdue receipts |
| | Member | Any member | Hide this functional button |
| Current Date vs Due Date?| Before due date | Today < Due Date | Maintain "Borrowed" status |
| | On due date | Today = Due Date | Mark as "Overdue" |
| | After due date | Today > Due Date | Mark as "Overdue" |
| Member views overdue? | Has overdue receipt | MEM002 | Sees BR001 marked as "Overdue" |
| | None | MEM006 | No overdue receipts displayed |
| Librarian views overdue?| After scanning | Librarian | Sees **all** overdue receipts |

---

### IDM - REQ-07: Member Management (Add New)

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Executed by role? | Librarian | `librarian@library.com` | Show form, execution permitted |
| | Member | `ba.nguyen@email.com` | Hide "Member" tab |
| Email valid? | Valid | `new@email.com` | Accepted |
| | Missing `.` in domain| `new@domain` | Rejected, error: invalid email |
| | Missing `@` | `newdomain.com` | Rejected, error: invalid email |
| | Empty | `""` | Rejected, error: required field |
| Email unique? | Does not exist | `brandnew@email.com` | Added successfully |
| | Already exists | `ba.nguyen@email.com` | Rejected, error: email already exists |
| Input Full Name? | Entered | `"Nguyễn Văn A"` | Accepted |
| | Empty | `""` | Rejected, error: required field |
| Input Phone Number? | Entered | `"0912345678"` | Accepted |
| | Empty | `""` | Rejected, error: required field |

---

### IDM - REQ-08: Borrow Receipt Lookup

| Characteristic | Partition | Representative Value | Expected Result |
|---|---|---|---|
| Looked up by role? | Librarian | `librarian@library.com` | Allowed to view **all** receipts |
| | Member | `ba.nguyen@email.com` | Only allowed to view receipts of MEM002 |
| View others' receipts? | Intentional lookup | MEM002 views MEM006's| **Not allowed** |
| Receipt details complete?| Borrowing | BR001 | Show ID, Book, Date, "Borrowed" |
| | Returned | BR002 | Show "Returned" |
| | Overdue | BR001 | Show "Overdue" |

---

## Step 2: Test Cases

### Tester 2 — REQ 01 and REQ 07 

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
| TC-01 | Verify successful login with a valid registered account | 1. User is not logged in.<br><br>2. Account exists and is activated in the system. | 1. Access login page.<br><br>2. Enter valid email.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: admin123 | 1. Redirect to homepage successfully.<br><br>2. App bar correctly displays user name and role: Nguyễn Thủ Thư (Librarian). | REQ-01 | EP |
| TC-02 | Verify error message when leaving both email and password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Leave both email and password blank.<br><br>3. Click Login. | Leave both fields blank. | System displays error message: "Please enter email and password". | REQ-01 | Decision Table | 
| TC-03 | Verify error message when entering correct email but leaving password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Enter correct email.<br><br>3. Leave password blank.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: (blank) | System blocks login and displays error message requiring password (or shares the blank field notification). | REQ-01 | BVA |
| TC-04 | Verify error message when entering correct email but incorrect password | 1. User is not logged in.<br><br>2. Account exists in the system. | 1. Access login section of the website.<br><br>2. Enter valid email.<br><br>3. Enter incorrect password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: hehe | System displays error message: "Incorrect password". | REQ-01 | EP | 
| TC-05 | Verify login behavior when intentionally capitalizing the first letter of the email | 1. User is not logged in.<br><br>2. Account librarian@library.com exists in the system. | 1. Go to login page.<br><br>2. Enter email with capitalized first letter.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: Librarian@library.com<br><br>2. Password: admin123 | System automatically converts email to lowercase, logs in successfully, and redirects to homepage. | REQ-01 | EP |
| TC-06 | Verify the security/masking of the password input field | User is on the login page. | Type characters into password field. | Password: admin123 | Entered characters must be masked as dots (●●●●●) or asterisks (*****). | REQ-01 | Black-box Testing |
| TC-07 | Verify error message when email format is invalid | User is not logged in. | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: ngochanhiepdepzai<br><br>2. Password: any | System displays invalid email format error message and does not send request to server. | REQ-01 | EP |
| TC-08 | Verify successful member addition with Librarian permission | Logged in with Librarian role. | 1. Fill in all valid information.<br><br>2. Click button: Add Member. | 1. Full Name: Ngô Chấn Hiệp<br><br>2. Email: ngohiep010605@gmail.com<br><br>3. Phone: 0941898905 | New member is created successfully and saved in the system, displaying: "Member added successfully! ID: MEMxxx". | REQ-07 | EP |
| TC-09 | Verify permission block when role is not Librarian | Logged in with a non-Librarian account (e.g., Member). | 1. Log in with non-librarian role account.<br><br>2. Observe interface. | 1. Email: dam.tran@email.com<br><br>2. Password: password123 | 1. Redirect to login/home page.<br><br>2. Appbar shows full information: Trần Dựa Dẫm (Member).<br><br>3. "Add Member" button is not visible. | REQ-07 | EP |
| TC-10 | Verify behavior when leaving all mandatory fields blank | Logged in with Librarian role. | 1. Leave Full Name, Email, and Phone fields blank.<br><br>2. Click Add Member. | All fields blank. | System prevents data submission and displays error prompts for mandatory fields. | REQ-07 | Decision Table |
| TC-11 | Add member with invalid email format (Has @ but missing dot .) | Logged in with Librarian role. | 1. Fill in info with email missing a dot in the domain section.<br><br>2. Click Add Member. | 1. Full Name: ngo chan hiep<br><br>2. Email: ghiep342@gmailcom<br><br>3. Phone: 0941898905 | System displays invalid email error message, registration denied. | REQ-07 | EP |
| TC-12 | Add member with invalid email format (Missing @ but has dot .) | Logged in with Librarian role. | 1. Fill in info with email missing @ symbol but having domain dot.<br><br>2. Click Add Member. | 1. Full Name: toi ten la tao<br><br>2. Email: ghiep242.com<br><br>3. Phone: 0941898905 | System displays invalid email error message, registration denied. | REQ-07 | EP | 
| TC-13 | Verify behavior when creating a member with an already existing email | Logged in with Librarian role. | 1. Log in as Librarian.<br><br>2. Use an email already existing on the system (dam.tran@email.com). | 1. Full Name: Ngô Chấn Hiệp<br><br>2. Email: dam.tran@email.com<br><br>3. Phone: 0941898905 | System blocks and displays duplication error message (e.g., "This email already exists"). | REQ-07 | EP |

---

### Tester 3 — In charge of User Module & Authentication

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|

---

### Tester 4 — In charge of Logic & Advanced Business Rules

**Decision Table (REQ-05):**
| Condition / Action | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|-----------------------|--------|--------|--------|--------|
| **Account Active?** | Yes | Yes | No | No |
| **Book Available?** | Yes | No | Yes | No |
| **Allow Borrowing?** | **YES** | **NO** | **NO** | **NO** |

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|

---

### Tester 5 - QA Analyst & Summary Report

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|

---

## Summary

| Functional Group | No. of TCs | Covered REQ | Applied Technique |
|------------------|------------|-------------|-------------------|
| Tester 2 | 6 | REQ-03, REQ-04 | BVA |
| Tester 3 | 6 | REQ-01, REQ-07 | EP |
| Tester 4 | 6 | REQ-02, REQ-05 | Decision Table |
| Tester 5 | 6 | REQ-06, REQ-08 | N/A |
| **Total** | **24** | **Full Coverage** | |



