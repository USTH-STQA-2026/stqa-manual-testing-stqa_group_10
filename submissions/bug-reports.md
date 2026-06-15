# Bug Reports

> **Guidelines**: Create 1 bug entry for each TC that has a **Fail** result.
> Each bug report requires: a descriptive title of the error behavior, reproduction steps, expected vs. actual results, and severity with an explanation.

| Information | |
|---|---|
| **Group** | Group 10 |
| **Reporting date** | 04/06/2026 |

---

## BUG-01: The system displays an incorrect error message and uses incorrect validation logic when the user enters an invalid email format on the login screen

| Attribute       | Details             |
| --------------- | ------------------- |
| **Bug ID** | BUG-01              |
| **Related TC** | TC-07, TC-08, TC-09 |
| **Related REQ** | REQ-01              |
| **Severity** | Medium              |
| **Reported By** | Ngo Chan Hiep       |
| **Date Found** | 28/05/2026          |
| **Status** | Open                |

### Steps to Reproduce

#### Scenario 1 (TC-07)
1. Open the login page.
2. Enter email: `librarianlibrarycom`
3. Enter any password.
4. Click **Login**.

#### Scenario 2 (TC-08)
1. Open the login page.
2. Enter email: `librarianlibrary.com`
3. Enter password: `admin123`
4. Click **Login**.

#### Scenario 3 (TC-09)
1. Open the login page.
2. Enter email: `librarian@librarycom`
3. Enter password: `admin123`
4. Click **Login**.

### Expected Result
For all invalid email formats:
* The system should validate the email format before sending a request to the server or querying the database.
* The system should display an error message such as:
  * **"Invalid email format"**
  * or **"Email is invalid"**
* The login process should be stopped until a valid email format is entered.

### Actual Result
For all three scenarios, the system displays:
**"Member not found"** (or **"Can not find member"**) instead of indicating that the email format is invalid.

### Impact
* Users may believe that the account does not exist rather than understanding that the email format is incorrect.
* The error message is misleading and does not help users identify the actual input problem.
* The system performs unnecessary account lookup operations before validating user input.

### Evidence
![BUG-01](./images/REQ-01/BUG-01.1.png)
![BUG-01](./images/REQ-01/BUG-01.2.png)
![BUG-01](./images/REQ-01/BUG-01.3.png)

### Proposed Fix
* Implement client-side and/or server-side email format validation before checking account existence.
* Use a standard email validation regex or a trusted validation library.
* When the email format is invalid:
  * Stop the login process immediately.
  * Display the message **"Invalid email format"**.
* Perform database lookup only after the email format passes validation.

---

## BUG-02: Email validation logic issue – valid emails are rejected while invalid emails (missing ".") are accepted

| Attribute       | Details                   |
| --------------- | ------------------------- |
| **Bug ID** | BUG-02                    |
| **Related TC** | TC-44                     |
| **Related REQ** | REQ-07                    |
| **Severity** | High                      |
| **Date Found** | 29/05/2026                |
| **Status** | Open                      |

### Environment
* Browser: Chrome
* Operating System: Windows 11
* Interface Language: Vietnamese

### Preconditions
* Logged in with a Librarian account.
* The Add New Member page is open.

### Steps to Reproduce
1. Open the Add New Member page.
2. **Case 1**:
   * Enter all valid information.
   * Use a valid email: `ngohiep010605@gmail.com`.
   * Click **Add Member**.
3. **Case 2**:
   * Enter all required information.
   * Use an invalid email that contains "@" but is missing "." in the domain: `ghiep342@gmailcom`.
   * Click **Add Member**.

### Expected Result
1. **Case 1**: The member is created successfully, data is saved, and a success message is displayed.
2. **Case 2**: The system should block the request and display the message: **"Invalid email"**.

### Actual Result
1. **Case 1**: The system blocks the request and displays **"Invalid email"**.
2. **Case 2**: The system successfully creates a new member with the invalid email `ghiep342@gmailcom`.

### Impact
This violates a core business rule for email validation. Users who enter correct emails cannot register, while invalid emails can be saved into the database, creating invalid records.

### Evidence
![BUG-02](./images/REQ-07/BUG-02.1.png)
![BUG-02](./images/REQ-07/BUG-02.2.png)

### Proposed Fix
* Review and fix the email validation logic in the Add Member feature.
* Ensure the system accepts valid emails that follow common standards (e.g., `username@gmail.com`).
* Ensure the system rejects invalid emails, such as those missing a domain section or missing a "." in the domain (e.g., `ghiep342@gmailcom`).
* Perform regression testing for all email validation scenarios after the fix.
* Use the same email validation rules across both Login and Add Member features to ensure consistent behavior.

---

## BUG-03: Adding a member with an existing email shows the wrong error message ("Invalid email" instead of "Email already exists")

| Attribute       | Details                   |
| --------------- | ------------------------- |
| **Bug ID** | BUG-03                    |
| **Related TC** | TC-42, TC-47              |
| **Related REQ** | REQ-07                    |
| **Severity** | High                      |
| **Reported By** | Ngo Chan Hiep             |
| **Date Found** | 29/05/2026                |
| **Status** | Open                      |

### Environment
* Browser: Chrome
* Operating System: Windows 11
* Interface Language: Vietnamese

### Preconditions
1. Logged in with a Librarian account.
2. An account with the email `dam.tran@email.com` already exists in the system.

### Steps to Reproduce
1. Open the Add New Member page.
2. Enter a valid Full Name and Phone Number.
3. Enter the existing email: `dam.tran@email.com`.
4. Click **Add Member**.

### Expected Result
The system should detect the duplicate email, prevent account creation, and display the message:
**"Email already exists in the system"**

### Actual Result
The system prevents account creation but displays the message:
**"Invalid email"**

### Impact
This violates the business requirement for error messages. The incorrect message may cause users or librarians to think the email format is wrong, while the actual issue is that the email has already been registered.

### Evidence
![BUG-03](./images/REQ-07/BUG-03.png)

### Proposed Fix
* Adjust the validation flow as follows:
  1. Check required fields.
  2. Validate email format.
  3. Check whether the email already exists.
  4. Create the account.
* When a duplicate email is detected, display the correct message: **"Email already exists in the system"**.
* Separate email format validation errors from duplicate email errors so each issue has its own clear message.

---

## BUG-04: The Add Member feature only displays the "Full Name" error message when the entire form is left blank

| Attribute       | Details       |
| --------------- | ------------- |
| **Bug ID** | BUG-04        |
| **Related TC** | TC-44         |
| **Related REQ** | REQ-07        |
| **Severity** | Low           |
| **Reported By** | Ngo Chan Hiep |
| **Date Found** | 29/05/2026    |
| **Status** | Open          |

### Environment
* Browser: Chrome
* Operating System: Windows 11
* Interface Language: Vietnamese

### Preconditions
* Logged in with a Librarian account.
* The Add New Member page is open.
* All input fields are empty.

### Steps to Reproduce
1. Leave all required fields empty (Full Name, Email, Phone Number).
2. Click **Add Member**.

### Expected Result
The system should block the request and display validation messages for all required fields that are empty (Full Name, Email, and Phone Number) so the user can correct them at once.

### Actual Result
The system only displays one validation message:
**"Full Name cannot be empty"** (or **"Fullname is not blank"**). No validation message is displayed for the Email or Phone Number fields, even though they are also empty.

### Impact
This negatively affects the user experience (UX). Users must repeatedly click the **Add Member** button because new validation errors only appear after fixing the previous one, which wastes time and causes frustration.

### Evidence
![BUG-04](./images/REQ-07/BUG-04.png)

### Proposed Fix
* Update the validation mechanism to check all required fields in a single submission.
* Display validation messages for all invalid fields at the same time instead of stopping at the first error.
* Highlight each invalid field visually (e.g., red border or warning icon) to help users identify and fix issues more easily.

---

## BUG-05: Search keyword is ignored when category is selected before searching

| Attribute           | Details                |
| ------------------- | ---------------------- |
| **Bug ID** | BUG-05                 |
| **Related TC** | TC-24, TC-27           |
| **Related REQ** | REQ-03                 |
| **Severity** | Medium                 |
| **Reporter** | Lê Đắc Duy - 23BA14084 |
| **Date Discovered** | 02/06/2026             |
| **Status** | Open                   |

### Environment
* Browser: Chrome
* OS: Windows 11
* Interface Language: Vietnamese

### Prerequisites
* The user has logged in successfully.
* The user is currently on the `Books` tab.
* The system has the category `Công nghệ`.
* The book list contains books in the `Công nghệ` category, including `Nhập môn lập trình Python`.

### Steps to Reproduce

#### Case 1 (TC-24)
1. Enter the keyword `Python` in the search box.
2. Select the category `Công nghệ`.
3. Observe the displayed book list.

#### Case 2 (TC-27)
1. Clear the search box and category filter.
2. Select the category `Công nghệ`.
3. Enter the keyword `Python` in the search box.
4. Observe the displayed book list.

### Expected Result
In both cases, the system should display the same result list. The displayed books must satisfy both conditions:
* The book belongs to the `Công nghệ` category.
* The book title or author contains the keyword `Python`.
*(e.g., the system should only display the book `Nhập môn lập trình Python` or show "No book found" when no items match).*

### Actual Result
* In Case 1 (TC-24), the system fails to parse combined filtering correctly and returns a mismatch/blank unexpected view or fails validation.
* In Case 2 (TC-27), when the user selects `Công nghệ` first and then enters `Python`, the system completely ignores the keyword and displays all books under the `Công nghệ` category.

### Impact
The Search & Filter function gives inconsistent results depending on the input order. Users may receive unrelated books when they select a category before entering a search keyword, making the filter feature unreliable.

### Evidence
![BUG-05 Case 1](./images/REQ-03/BUG-05.1.png)
![BUG-05 Case 2](./images/REQ-03/BUG-05.2.png)

### Proposed Fix
* Recalculate the book list whenever either the search keyword or category filter changes.
* Apply both search keyword and category filter together using AND logic.
* Ensure the result is consistent regardless of input order.

---

## BUG-08: System allows member to borrow 4 books concurrently (Off-by-one boundary limit bypass)

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | BUG-08                                                       |
| **Related TC** | TC-29                                                        |
| **Related REQ** | REQ-04                                                       |
| **Severity** | **High** — Violates core business rules by allowing members to borrow books beyond the maximum limit. |
| **Reported By** | Nguyễn Văn Hoàng - 23BA14122                                 |
| **Date Found** | 05/06/2026                                                   |
| **Status** | Open                                                         |

### Environment
* Browser: Chrome Version 148.0.7778.217
* OS: Windows 11
* Interface Language: Vietnamese

### Preconditions
* Member account `"ba.nguyen@email.com"` is logged in.
* The account currently has exactly 1 active borrowed book record in the system.

### Steps to Reproduce
1. Navigate to the "Books" tab.
2. Click the `(+)` button to borrow book `BOOK001` (Total active borrows = 2).
3. Click the `(+)` button to borrow book `BOOK002` (Total active borrows = 3).
4. Attempt to borrow a 4th book (`BOOK005`) by clicking its `(+)` button.

### Expected Result
At step 4, the system must block the action and display a clear error message stating: "Maximum limit of 3 books reached" to prevent the 4th book from being borrowed.

### Actual Result
At step 4, the system allows the borrow action for `BOOK005` to process successfully. The member's total active borrowed books increases to 4 without any warning or restriction.

### Impact
Allows users to bypass the business rule constraint. It breaks the library inventory workflow, disrupts book availability tracking, and negatively impacts other members' borrowing privileges.

### Evidence
![Bug 08 Evidence](./images/REQ-04/BUG_08.png)

### Proposed Fix
Verify the comparison operator inside the active borrows validation logic. Ensure the system restricts the borrow action if `active_borrows >= 3` instead of an incorrect condition like `< 4` or `<= 3`.

---

## BUG-09: "Suspended" member account displays incorrect error message stating "Thành viên đã hết hạn" upon borrowing

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | BUG-09                                                       |
| **Related TC** | TC-30                                                        |
| **Related REQ** | REQ-04                                                       |
| **Severity** | **High** — System misidentifies user core account state and displays an incorrect message. |
| **Reported By** | Nguyễn Văn Hoàng 23BA14122                                   |
| **Date Found** | 05/06/2026                                                   |
| **Status** | Open                                                         |

### Environment
* Browser: Chrome 148.0.7778.217
* OS: Windows 11
* Interface Language: Vietnamese

### Preconditions
* Member account `"cu.le@email.com"` is logged in (Account status is Suspended in Seed Data).

### Steps to Reproduce
1. Navigate to the "Books" tab.
2. Find any book that is currently marked as "Available" ("Có sẵn").
3. Click the `(+)` button to attempt to borrow the book.

### Expected Result
The system blocks the action and displays a specific error message reflecting that the account is currently "Suspended / Temporarily Disabled".

### Actual Result
The system blocks the action but displays an incorrect, unrelated red error banner at the bottom stating: "Thành viên đã hết hạn. Không thể mượn sách." (Member has expired. Cannot borrow book).

### Impact
The system misidentifies the user state workflow. It misleads suspended members about the actual reason their features are locked, making it difficult for administrators or librarians to handle user inquiries accurately.

### Evidence
![Bug 09 Evidence](./images/REQ-04/BUG_09.png)

### Proposed Fix
Check the error handling logic or conditional flow (`switch/case` or `if/else`) validating member status during borrowing. Ensure that the error code returned for a `Suspended` account maps to its correct UI string instead of falling back to the `Expired` account message string.

---

## BUG-10: System does not display overdue book return warnings when returns are overdue

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | `BUG-10`                                                     |
| **Related TC** | `TC-39`                                                      |
| **Related REQ** | `REQ-05`                                                     |
| **Severity** | `High`                                                       |
| **Reported By** | `Nguyễn Văn Hoàng`                                           |
| **Date Found** | `07/06/2026`                                                 |
| **Status** | `Open`                                                       |

### Environment
* Browser: Chrome `Version 149.0.7827.54`
* OS: `Windows 11`
* UI Language: `English & Vietnamese`

### Prerequisites
* Account successfully logged in, at least one book is overdue for return in the borrowing list.

### Steps to Reproduce
1. Go to the "Borrowed Books" section.
2. Confirm that there are books that are overdue. Click "Return Book" on that overdue book.
3. Confirm returning the book.

### Expected Result
After returning, the system must display a warning notification or text alert such as: "Book is overdue by X days, you may be fined" so users are fully aware.

### Actual Result
The book return process finishes normally; no warnings or penalty notices are displayed on the screen interface.

### Impact
Users are unaware of being fined, leading to surprise and subsequent complaints. Librarians have no explicit prompt basis to process fines immediately because the client interface completely suppresses it, affecting transparency.

### Evidence
![BUG-10](./images/REQ-05/BUG-10.png)

### Proposed Solution
Add a warning modal/popup before confirming overdue book returns, displaying the number of days late and the corresponding penalty fee. Ensure the backend calculates and attributes the penalty record history properly.