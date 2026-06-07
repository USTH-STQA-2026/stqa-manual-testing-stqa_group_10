## Step 1: Input Domain Modeling — Input Domain Modeling (IDM)

# Test Cases — Test Case Table

| Information | |
|---|---|
| **Team** | Team 10 |
| **Created date** | 26/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |


### IDM - REQ-01: Login 

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Is the email present in DB? | Yes | `librarian@library.com` | Login successful |
|  | No | `noone@email.com` | Error: "Account does not exist" |
| Is the password correct? | Correct | `admin123` | Login successful |
|  | Incorrect | `wrongpass` | Error: "Incorrect password" |
| Is the input field empty? | Not empty | (any) | Normal handling |
|  | Empty | `""` | Error: "Please enter email and password" |

---

### IDM - REQ-02: View Book List

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Account role? | Librarian | `librarian@library.com` | See all books with full details |
|  | Member | `ba.nguyen@email.com` | See all books with full details |
| Book status? | Available | BOOK001, BOOK002 | Display "Available" |
|  | Borrowed | BOOK003 | Display "Borrowed" |
|  | Lost | BOOK007 | Display "Lost" |
| Complete information? | 5 fields present | BOOK001 | Show Title, Author, Category, Year, Status |
| Realtime update? | After borrowing | Borrow BOOK001 | Status immediately changes to "Borrowed" |
|  | After returning | Return BOOK003 | Status immediately changes to "Available" |

---

### IDM - REQ-03: Search & Filter

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Keyword exists? | Yes (Book title) | `"Flutter"` | Show books containing "Flutter" |
|  | Yes (Author) | `"Nguyễn"` | Show books by author "Nguyễn" |
|  | No | `"XYZ123"` | Show "No books found" message |
| Case sensitivity? | Lowercase | `"flutter"` | Results match "Flutter" |
|  | Uppercase | `"FLUTTER"` | Results match "Flutter" |
| Filter by category? | Category has books | Select `"Technology"` | Show only Technology books |
|  | Category has no books | Empty/None | Show "No books found" message |

---

### IDM - REQ-04: Borrow Book

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Book status? | Available | BOOK001 | Allow borrowing |
|  | Borrowed | BOOK003 | Deny with error |
|  | Lost | BOOK007 | Deny with error |
| Member status? | Active | MEM002 | Allow borrowing |
|  | Suspended | MEM004 | Deny, show account suspended error |
|  | Expired | MEM005 | Deny, show account expired error |
| Number of currently borrowed books? | < 3 | MEM006 (1 book) | Allow borrowing |
|  | = 3 | Member has 3 borrows | Deny, show limit exceeded error |

---

### IDM - REQ-05: Return Book

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Does the receipt belong to the user? | Yes | MEM002 returns BOOK003 | Allow return |
|  | No | MEM002 returns BOOK013 | Hide return option / Deny |
| Timeliness? | Within due date | BR003 | Return successful, no warning |
|  | Overdue | BR001 | Return successful AND show overdue warning |
|  | Already returned | BR002 | Hide return option |
| Book status after return? | Successful | BOOK003 | Change to "Available" |
| Receipt status after return? | Successful | BR001 | Change to "Returned" |

---

### IDM - REQ-06: Overdue Handling

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Who triggers? | Librarian | `librarian@library.com` | Scan and update overdue records |
|  | Member | Any member | Hide this function button |
| Current date vs Due date? | Before due | Today < Due date | Keep status "Borrowed" |
|  | On due date | Today = Due date | Mark as "Overdue" |
|  | After due date | Today > Due date | Mark as "Overdue" |
| Member sees overdue? | Has overdue receipts | MEM002 | Sees BR001 marked "Overdue" |
|  | No overdue | MEM006 | Sees no overdue receipts |
| Librarian sees overdue? | After scanning | Librarian | Sees all overdue receipts |

---

### IDM - REQ-07: Manage Members (Add New) 

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Who can perform? | Librarian | `librarian@library.com` | Show form and can perform add |
|  | Member | `ba.nguyen@email.com` | Hide "Members" tab |
| Email valid? | Valid | `new@email.com` | Accept |
|  | Missing `.` in domain | `new@domain` | Reject, show invalid email |
|  | Missing `@` | `newdomain.com` | Reject, show invalid email |
|  | Empty | `""` | Reject, show required field error |
| Email unique? | Not existing | `brandnew@email.com` | Add successfully |
|  | Already exists | `ba.nguyen@email.com` | Reject, show email already exists |
| Full name entered? | Entered | `"Nguyễn Văn A"` | Accept |
|  | Empty | `""` | Reject, show required field error |
| Phone number entered? | Entered | `"0912345678"` | Accept |
|  | Empty | `""` | Reject, show required field error |

---

### IDM - REQ-08: Borrow Receipt Lookup

| Feature | Partition | Representative value | Expected result |
|---|---|---|---|
| Who looks up? | Librarian | `librarian@library.com` | View **all** receipts |
|  | Member | `ba.nguyen@email.com` | View only receipts belonging to MEM002 |
| View another member's receipts? | Attempt to view others | MEM002 views MEM006 | **Not allowed** |
| Receipt information complete? | Borrowed | BR001 | Show ID, Book, Date, "Borrowed" |
|  | Returned | BR002 | Show "Returned" |
|  | Overdue | BR001 | Show "Overdue" |

## Step 2: Test Cases

### REQ-01 : Login

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
| TC-01 | Verify successful login with a valid registered account | 1. User is not logged in.<br><br>2. Account exists and is activated in the system. | 1. Access login page.<br><br>2. Enter valid email.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: admin123 | 1. Redirect to homepage successfully.<br><br>2. App bar correctly displays user name and role: Nguyễn Thủ Thư (Librarian). | REQ-01 | EP |
| TC-02 | Verify error message when leaving both email and password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Leave both email and password blank.<br><br>3. Click Login. | Leave both fields blank. | System displays error message: "Please enter email and password". | REQ-01 | Decision Table | 
| TC-03 | Verify error message when entering correct email but leaving password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Enter correct email.<br><br>3. Leave password blank.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: (blank) | System blocks login and displays an error message requiring a password (or shows blank-field notification). | REQ-01 | BVA |
| TC-04 | Verify error message when entering correct email but incorrect password | 1. User is not logged in.<br><br>2. Account exists in the system. | 1. Access login section of the website.<br><br>2. Enter valid email.<br><br>3. Enter incorrect password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: hehe | System displays error message: "Incorrect password". | REQ-01 | EP | 
| TC-05 | Verify login behavior when intentionally capitalizing the first letter of the email | 1. User is not logged in.<br><br>2. Account librarian@library.com exists in the system. | 1. Go to login page.<br><br>2. Enter email with capitalized first letter.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: Librarian@library.com<br><br>2. Password: admin123 | System automatically converts email to lowercase, logs in successfully, and redirects to homepage. | REQ-01 | EP |
| TC-06 | Verify the security/masking of the password input field | User is on the login page. | Type characters into the password field. | Password: admin123 | Entered characters must be masked as dots (●●●●●) or asterisks (*****). | REQ-01 | Black-box Testing |
| TC-07 | Verify error message when email format is invalid | User is not logged in. | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarianlibrarycom<br><br>2. Password: any | System displays invalid email format error message and does not send request to server. | REQ-01 | EP |
| TC-08 | Verify the input value of email | User is not logged in | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarianlibrary.com<br><br>2. Password: admin123 | System displays error message: "Email is invalid" | REQ-01 | EP |
| TC-09 | Verify the input value of email | User is not logged in | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarian@librarycom<br><br>2. Password: admin123 | System displays error message: "Email is invalid" | REQ-01 | EP |

### REQ-02 View Book List

| TC ID | Test Objective | Preconditions  | Execution Steps | Input Data | Expected Result | REQ    | Technique |
| ----- | -----------| ---------- | ----------| --------- | -----------| ------ | -------------- |
| TC-10 | Verify that the librarian can view the list of all books        | 1. User is not logged in.<br><br>2. The data is in the seed data state.               | 1. Open the system https://stqa.rbc.vn.<br><br>2. Log in with the librarian account.<br><br>3. Select the Books tab.<br><br>4. Observe the displayed book list.                             | 1. Email: [librarian@library.com](mailto:librarian@library.com)<br><br>2. Password: admin123 | The system displays the list of all books in the library with complete information: book title, author, category, publication year, book code, and status. | REQ-02 | EP             |
| TC-11 | Verify that a member can view the list of all books             | 1. User is not logged in.<br><br>2. The data is in the seed data state.               | 1. Open the system https://stqa.rbc.vn.<br><br>2. Log in with the member account.<br><br>3. Select the Books tab.<br><br>4. Observe the displayed book list.                                | 1. Email: [ba.nguyen@email.com](mailto:ba.nguyen@email.com)<br><br>2. Password: password123  | The member can view the book list with complete information: book title, author, category, publication year, book code, and status.  | REQ-02 | EP |
| TC-12 | Verify that a book with Available status is displayed correctly | User has logged in successfully. | 1. Select the Books tab.<br><br>2. Find the book BOOK001 - Lập trình Flutter cơ bản.<br><br>3. Check the book status.   | Book: BOOK001    | The book displays the status Available.    | REQ-02 | EP |
| TC-13 | Verify that a book with Borrowed status is displayed correctly  | User has logged in successfully.  | 1. Select the Books tab.<br><br>2. Find the book BOOK003 - Kiểm thử phần mềm nhập môn.<br><br>3. Check the book status. | Book: BOOK003 | The book displays the status Borrowed. | REQ-02 | EP |
| TC-14 | Verify that the book status is updated after borrowing   | 1. Member account is active.<br><br>2. Book BOOK001 is currently in Available status. | 1. Log in with the member account.<br><br>2. Select the Books tab.<br><br>3. Borrow book BOOK001.<br><br>4. Observe the book status after borrowing. | 1. Email: [ba.nguyen@email.com](mailto:ba.nguyen@email.com)<br><br>2. Book: BOOK001          | The book status is updated from Available to Borrowed immediately after borrowing successfully.                                                 | REQ-02 | Decision Table |
| TC-15 | Verify that the book status is updated after returning          | Member is currently borrowing book BOOK003.                                           | 1. Log in with the member account.<br><br>2. Select the Borrow / Return tab.<br><br>3. Return book BOOK003.<br><br>4. Go back to the Books tab.<br><br>5. Check the status of book BOOK003. | 1. Email: [ba.nguyen@email.com](mailto:ba.nguyen@email.com)<br><br>2. Book: BOOK003          | The book status is updated from Borrowed to Available after returning successfully. | REQ-02 | Decision Table |
| TC-16 | Verify that the publication year is displayed in a valid format | User has logged in successfully.  | 1. Select the Books tab.<br><br>2. Observe the publication year information of the books.  | None  | The publication year of each book is displayed in a valid 4-digit number format and is not empty.   | REQ-02 | BVA |

### REQ-03 Search & Filter

| TC ID | Test Objective | Preconditions   | Execution steps | Input Data | Expected Result | REQ    | Technique |
| ----- | ------ | ------| -------| ---- | ------ | ------ | -------- |
| TC-17 | Search for a book by a valid book title                     | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword Flutter.<br><br>3. Observe the search result list.             | Keyword: 'Flutter'                                  | The system displays books whose titles contain Flutter.                                        | REQ-03 | EP             |
| TC-18 | Search for a book title using lowercase letters             | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword flutter.<br><br>3. Observe the search result list.             | Keyword: 'flutter'                                  | The system still displays books containing the keyword Flutter.                                | REQ-03 | EP             |
| TC-19 | Search for a book title using uppercase letters             | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword FLUTTER.<br><br>3. Observe the search result list.             | Keyword: 'FLUTTER'                                  | The system displays the correct search result. Searching is not case-sensitive.                | REQ-03 | EP             |
| TC-20 | Search for books by author                                  | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the author name Nguyễn Minh Đức.<br><br>3. Observe the search result list. | Author: Nguyễn Minh Đức                           | The system displays books written by the corresponding author.                                 | REQ-03 | EP             |
| TC-21 | Search with a non-existing keyword                          | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword XYZ123.<br><br>3. Observe the displayed result.                | Keyword: 'XYZ123'                                   | The system displays the message No books found.                                                | REQ-03 | EP             |
| TC-22 | Combine search and category filter with matching results    | User has logged in successfully and is on the Books tab. | 1. Select the category Technology.<br><br>2. Enter the keyword Python.<br><br>3. Observe the displayed result.      | 1. Category: Technology<br><br>2. Keyword: Python | The system displays books that match both the search keyword and the selected category filter. | REQ-03 | Decision Table |
| TC-23 | Combine search and category filter with no matching results | User has logged in successfully and is on the Books tab. | 1. Select the category Economics.<br><br>2. Enter the keyword Flutter.<br><br>3. Observe the displayed result.      | 1. Category: Economics<br><br>2. Keyword: Flutter | The system displays the message No books found.                                                | REQ-03 | Decision Table |
| TC-24 | Verify the minimum search keyword length boundary           | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter one character in the search box.<br><br>3. Observe the displayed result.   | Keyword: 'F'                                        | The system still processes the search and displays matching results if available.              | REQ-03 | BVA            |

---

### REQ-04 Borrow Book

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
|TC-25|Verify successful book borrowing under limit (<= 3)|Logged in as `biet.hoang@email.com` (currently has 1 active borrow)| 1. Go to "Books" tab.<br><br>2. Click the "+" button for an "Available" book (`"BOOK001"`) then "Borrow".<br><br>3. Navigate to the "Borrow / Return" tab.<br><br>4. Observe the newly created borrow record |`"BOOK001"`|Displays a success message. A borrow record is created with a due date set to exactly +14 days from today|REQ-04|EP|
|TC-26|Verify borrow rejection at the boundary limit (= 3)|Logged in as `ba.nguyen@email.com` (currently has 1 active borrow)| 1. Borrow `"BOOK001"` (Total = 2).<br>2. Borrow `"BOOK002"` (Total = 3).<br>3. Attempt to borrow `"BOOK005"`|`"BOOK001"`, `"BOOK002"`, `"BOOK005"`|System allows the first 2 borrows. Rejects the 3rd attempt (`"BOOK005"`) with an error message stating the limit of 3 books is reached|REQ-04|BVA|
|TC-27|Verify borrow rejection for "Suspended" member|Logged in as `cu.le@email.com` (Member status: Suspended)|1. Go to "Books" tab.<br><br>2. Select an "Available" book (`"BOOK001"`).<br><br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Suspended" account status|REQ-04|Decision Table|
|TC-28|Verify borrow rejection for an already "Borrowed" book|Logged in as `dam.tran@email.com`|1. Go to "Books" tab.<br><br>2. Search or locate `"BOOK003"` (Status: Borrowed).<br><br>3. Observe the book card|`"BOOK003"`|The "+" borrow button is completely hidden/disabled for this book and status is "Borrowed"|REQ-04|EP|
|TC-29|Verify borrow rejection for "Expired" member|Logged in as `binh.pham@email.com` (Member status: Expired)|1. Go to "Books" tab.<br><br>2. Select an "Available" book (`"BOOK001"`).<br><br>3. Click the"+" button then "Borrow"|`"BOOK001"`|The system rejects the request and displays a specific error message for the "Expired" account status|REQ-04|EP|
|TC-30|Verify borrow rejection for "Lost" book|Member `"ba.nguyen@email.com"` is logged in|1. Locate `"BOOK007"` (Status: Lost).<br>2. Observe the book card|Book: `"BOOK007"`|The "+" borrow button is completely hidden/disabled and status is "Lost"|REQ-04|EP|


---

### REQ-05 Return Book

| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
|TC-31|Return a borrowed book on time, no overdue warning| Logged in ba.nguyen@email.com. Just borrowed BOOK001.|1. Go to tab "Borrow/Return".<br><br>2. Find BOOK001 in "My borrow records".<br><br>3. Click "Return book". Confirm if prompted |BOOK001, borrow date = today, due date = today + 14 days|1.Success message shown. 2.Record status "Returned". 3.BOOK001 status -> "Available". 4.No overdue warning|REQ-05|EP|
|TC-32|Return an overdue book - allowed but shows overdue warning | Logged in librarian@library.com, and Click `"Check overdue books"` <br> Logged in ba.nguyen@email.com. Record BR001 exists: MEM002 borrowed BOOK003, due 15/09/2024|1. Go to "Borrow/Return" tab.<br><br>2. Find "BR001" in "My Borrow records".<br><br>3. Click "Return book" |Record: BR001, BOOK003, due date: 15/09/2024, return date: today  |1. Return is accepted. 2. BR001 status -> "Returned". 3. BOOK003 status -> "Available" |REQ-05|EP |
|TC-33| Cannot return a book borrowed by another member |Logged in ba.nguyen@email.com. BOOK013 is borrowed by another member, not MEM002 |1. Go to "Borrow/Return" tab.<br><br>2. Find BOOK013 in "My borrow records" |Account of MEM002 but BOOK013 is borrowed by MEM006 |1. Cannot find BOOK013 in MEM002's list. 2. No "Return" button available for BOOK013. 3. System does not allow returning another member's book |REQ-05 |EP |
|TC-34| Book status updates immediately after return| Logged in as ba.nguyen@email.com.  BOOK003 currently shows "Borrowed" in Books tab|1. Go to "Borrow/Return".<br><br>2. Find BR001 in "My borrow records".<br><br>3. Click "Return book".<br><br>4. Immediately go back to "Books" tab.<br><br>5. Find BOOK003 |Record:BR001, BOOK003 |1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" — updates immediately |REQ-05 |EP |
| TC-35 | Check the feature to block duplicate requests (spam clicks) when returning books | Log in using your Librarian account | Step 1: Go to the Borrow/Return tab.<br><br>Step 2: Find an active loan slip.<br><br>Step 3: Click the Return button repeatedly and observe the quantity and content of the popup that appears on the screen | Ticket code: `BR003` and the action of clicking repeatedly | The system only accepts and processes the first click request, displaying only one popup message: "Book returned successfully".     | REQ-05 | EP |
| TC-36 | Check if the overdue warning is displayed when the book return is overdue | Log in using your member account  | Step 1: Go to the Borrow/Return tab.<br><br>Step 2: Check if the system displays any overdue warnings  | Loan slip code and book code  | The system is required to display a clear overdue warning on the interface screen  | REQ-05 | EP |

---

### REQ-06 Overdue Handling
| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-37 | Check overdue books | A borrow record has `dueDate` ≤ current date<br> (Currently 2 overdue book) | 1. Log in with Librarian account.<br><br>2. Select 'Borrow/Return' tab.<br><br>3. Click 'Check overdue books' button. | Librarian account | 1. Notification 'updated: 2 overdue book' overdue books.<br><br>2. Book statuses change from 'Borrowed' to 'Overdue'. | REQ-06 | STT |
| TC-38 | Member sees their overdue receipts | A borrow record has `dueDate` ≤ current date | 1. Log in with Member account.<br><br>2. Select 'Borrow/Return' tab. | Member account | Book status for borrowed items changes from 'Borrowed' to 'Overdue' | REQ-06 | STT |

### REQ-07 Manage Member (Add new)
| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
| TC-39 | Verify successful member addition with Librarian permission | Logged in with Librarian role. | 1. Fill in all valid information.<br><br>2. Click button: Add Member. | 1. Full Name: Ngô Chấn Hiệp<br><br>2. Email: ngohiep010605@gmail.com<br><br>3. Phone: 0123456789 | New member is created successfully and saved in the system, displaying: "Member added successfully! ID: MEMxxx". | REQ-07 | EP |
| TC-40 | Verify permission block when role is not Librarian | Logged in with a non-Librarian account (e.g., Member). | 1. Log in with non-librarian role account.<br><br>2. Observe interface. | 1. Email: dam.tran@email.com<br><br>2. Password: password123 | 1. Redirect to login/home page.<br><br>2. Appbar shows full information: Trần Dựa Dẫm (Member).<br><br>3. "Add Member" button is not visible. | REQ-07 | EP |
| TC-41 | Verify behavior when leaving all mandatory fields blank | Logged in with Librarian role. | 1. Leave Full Name, Email, and Phone fields blank.<br><br>2. Click Add Member. | All fields blank. | System prevents data submission and displays error prompts for mandatory fields. | REQ-07 | Decision Table |
| TC-42 | Add member with invalid email format (Has @ but missing dot .) | Logged in with Librarian role. | 1. Fill in info with email missing a dot in the domain section.<br><br>2. Click Add Member. | 1. Full Name: ngo chan hiep<br><br>2. Email: ghiep342@gmailcom<br><br>3. Phone: 0123456789 | System displays invalid email error message, registration denied. | REQ-07 | EP |
| TC-43 | Add member with invalid email format (Missing @ but has dot .) | Logged in with Librarian role. | 1. Fill in info with email missing @ symbol but having domain dot.<br><br>2. Click Add Member. | 1. Full Name: toi ten la tao<br><br>2. Email: ghiep242.com<br><br>3. Phone: 0941898905 | System displays invalid email error message, registration denied. | REQ-07 | EP | 
| TC-44 | Verify behavior when creating a member with an already existing email | Logged in with Librarian role. | 1. Log in as Librarian.<br><br>2. Use an email already existing on the system (dam.tran@email.com). | 1. Full Name: Ngô Chấn Hiệp<br><br>2. Email: dam.tran@email.com<br><br>3. Phone: 0941898905 | System blocks and displays duplication error message (e.g., "This email already exists"). | REQ-07 | EP |
| TC-45 |Check that the list of valid members with the role of Librarian is displayed.| 1. The librarian's account has successfully logged in. <br><br> 2. Member data is now available in the system. | 1. Navigate to the "Member" menu.<br><br> 2. Observe the displayed list.| Empty | 1. The screen displays a list of members.<br><br> 2. All columns are displayed: Serial Number, Full Name, Email, Phone Number , Number of books currently borrowed.<br><br> 3. The data matches the database accurately.| REQ-07 | Black-box Testing |

---

### REQ-08 - Borrow Receipt Lookup
| TC ID | Test Objective | Preconditions | Execution Steps | Input Data | Expected Result | REQ | Technique |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-46 | Librarian views all borrow receipts of all members | Members have borrowed books from the library | 1. Log in with Librarian account<br><br>2. Select 'Borrow/Return' tab | Librarian account | The 'all receipts' tab shows a list of borrow receipts with book title, borrower name, borrow date, due date, and the book's borrow status. | REQ-08 | EP |
| TC-47 | Member views all their own borrow receipts | Member has borrowed books from the library | 1. Log in with Member account<br><br>2. Select 'Borrow/Return' tab | Member account | The 'My borrow receipts' tab shows all receipts that belong to the logged-in member, including book title, receipt ID, member name, borrow date, due date, and book status. | REQ-08 | EP |


## Summary

| Functional area | # of TCs | REQs covered | IDM techniques applied |
|----------------|------:|-------------|----------------------|
| Login | 9 | REQ-01 | EP, BVA, Decision Table, Black-box Testing |
| View Book List | 7 | REQ-02 | EP, Decision Table, BVA |
| Search & Filter | 8 | REQ-03 | EP, Decision Table, BVA |
| Borrow Book | 6 | REQ-04 | EP, BVA, Decision Table |
| Return Book | 6 | REQ-05 | EP |
| Overdue Handling | 2 | REQ-06 | State Transition Testing (STT) |
| Manage Members | 7 | REQ-07 | EP, Decision Table |
| Borrow Receipt Lookup | 2 | REQ-08 | EP |
| **Total** | **47** | REQ-01..REQ-08 | EP, BVA, Decision Table, STT, Black-box Testing |
