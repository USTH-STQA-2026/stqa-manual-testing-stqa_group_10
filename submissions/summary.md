# Test Summary — Test Summary Report

## 1. Team information

| Item | Detail |
|------|--------|
| **Team** | `STQA Group 10` |
| **Class** | `252ICT2012.L1` |
| **Report date** | `07/06/2026` |
| **Tested system** | https://stqa.rbc.vn — v1.0 |

---

## 2. Overall results

*Note: The following metrics reflect the actual execution of all 50 test cases recorded in `test-execution.md`.*

| Metric | Value |
|--------|-------|
| Total test cases | `50` |
| Pass | `39` |
| Fail | `11` |
| Blocked | `0` |
| Not Run | `0` |
| **Pass rate** | `78.00%` |
| **Bugs found** | `7` |

### Distribution by functional area

| Functional area | # TC | Pass | Fail | Blocked | Notes |
|-----------------|-----:|------|------|---------|-------|
| REQ-01: Login | 9 | 5 | 4 | 0 | Pass rate: 55.56% |
| REQ-02: View Book List | 9 | 9 | 0 | 0 | Pass rate: 100.00% |
| REQ-03: Search & Filter | 10 | 8 | 2 | 0 | Pass rate: 80.00% |
| REQ-04: Borrow Book | 6 | 4 | 2 | 0 | Pass rate: 66.67% |
| REQ-05: Return Book | 6 | 5 | 1 | 0 | Pass rate: 83.33% |
| REQ-06: Overdue Handling | 2 | 2 | 0 | 0 | Pass rate: 100.00% |
| REQ-07: Manage Members (Add New) | 7 | 3 | 4 | 0 | Pass rate: 42.86% |
| REQ-08: Borrow Receipt Lookup | 2 | 2 | 0 | 0 | Pass rate: 100.00% |

### Bugs by severity

| Severity | Count | Bug IDs |
|---------|------:|---------|
| **High** | 4 | BUG-02, BUG-03, BUG-08, BUG-09, BUG-10 |
| **Medium** | 2 | BUG-01, BUG-05 |
| **Low** | 1 | BUG-04 |

*(Note: While there are 11 failed test cases, they consolidate into 7 actual unique bug reports because BUG-03 covers both TC-42 and TC-47, and BUG-05 covers both TC-24 and TC-27).*

---

## 3. Design techniques applied (IDM / test design)

| Technique | Applied to REQs | # of TCs (approx) | Notes |
|-----------|------------------|------------------:|-------|
| Equivalence Partitioning (EP) | REQ-01, REQ-02, REQ-03, REQ-04, REQ-05, REQ-07, REQ-08 | 41 | Widely used for validating valid and invalid input domains and data partitions. |
| Boundary Value Analysis (BVA) | REQ-01, REQ-02, REQ-03, REQ-04 | 4 | Applied to input string lengths and numeric rule limits. |
| Decision Table | REQ-01, REQ-02, REQ-03, REQ-04, REQ-07 | 4 | Used for complex logical combinations of business constraints and user roles. |
| State Transition Testing (STT) | REQ-06 | 2 | Used to map and test loan state changes (e.g., Active Loan → Overdue status). |
| Black-box Testing | REQ-01, REQ-07 | 2 | Applied for visual validation and layout behavior of the client application interfaces. |

---

## 4. Quality analysis

### 4.1 Strengths
* **REQ-02 (View Book List), REQ-06 (Overdue Handling), and REQ-08 (Borrow Receipt Lookup)** functions perfectly with an absolute 100% Pass rate. The system reliably displays complete book lists and processes historical log details without errors.
* Basic keyword search features work well when exact matches are provided, and the queries are correctly case-insensitive.
* Standard checkout/check-in operations run smoothly for typical accounts without active suspensions or constraints.

### 4.2 Weaknesses
* **System-wide email parsing and validation logic is severely broken**: The login page omits frontend email structure checks, causing unnecessary database lookup loads and returning misleading error notifications ("Member not found"). In the Add Member feature, the logic is inverted (it saves invalid strings missing "." but rejects completely valid email syntaxes).
* **Misleading and incorrect system message prompts**: Registering a duplicate email produces an "Invalid email" prompt instead of a duplicate alert. A user with a "Suspended" account attempting a loan is greeted with a confusing "Expired membership" text block.
* **Failure to enforce key Business Rules**: The Search & Filter combined feature executes inconsistently, ignoring keyword query strings entirely if a category dropdown filter is picked first. Shockingly, the platform permits book loans to bypass the maximum allowance threshold (allowing a 4th concurrent book check-out).
* **Suppressed transactional visibility**: Returning overdue books succeeds technically, but the client completely fails to trigger or display any fine computations or late-return warnings, which damages operating transparency.
* **Suboptimal User Experience (UX)**: The member registration form checks constraints one field at a time (e.g., only flashing one error for Full Name), forcing the librarian to click submit repeatedly to discover other empty required inputs.

---

## 5. Priority recommendations for fixes

Bug prioritization criteria are structured primarily by technical severity impact followed by risk to core library operations:

| Priority | Bug ID | Severity | Rationale |
|---------:|--------|---------:|-----------|
| **1** | **BUG-08** | High | Critical business logic violation. Bypasses core check-out constraints by allowing a member to borrow a 4th book, risking library stock control. |
| **2** | **BUG-02** | High | Severe input validation defect. Accepts broken, corrupt email addresses into the database while blocking completely legitimate formats. |
| **3** | **BUG-03** | High | Violates functional requirements. Misleads librarians into troubleshooting syntax errors instead of notifying them of an existing duplicated account profile. |
| **4** | **BUG-09** | High | Severe status mapping failure. Incorrectly flags a "Suspended" user account as "Expired," leading to confusion and administrative mishandling. |
| **5** | **BUG-10** | High | Zero visibility on financial penalties. Suppresses penalty warnings during late returns on the interface, creating transparency issues and operational friction. |
| **6** | **BUG-01** | Medium | Causes redundant backend data queries on malformed user strings and provides generic, inaccurate feedback for login failures. |
| **7** | **BUG-05** | Medium | Unreliable search results. Data filtering fails depending on the sequence in which fields are filled, compromising the core lookup module. |
| **8** | **BUG-04** | Low | Minor UX issue. Forces repetitive submissions to identify empty required parameters sequentially rather than validating the form holistically. |

---

## 6. Conclusion

The system under test is currently **NOT READY FOR PRODUCTION RELEASE**.

**Justification**: Although several database logging and data representation tables operate with a 100% success rate, the build is severely compromised by multiple **High-severity** defects. These issues breach fundamental library regulations (over-loaning allowances), fail core system validations (broken email parsing algorithms), and generate misleading account statuses and silent errors across the UI. Fixes for all High and Medium bugs must be deployed, followed by comprehensive Regression Testing, before production sign-off can be granted.