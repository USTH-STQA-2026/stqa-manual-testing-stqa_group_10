# Test Summary — Test Summary Report

> **Note**: This is a Quality Assurance summary — focus on overall software quality, not only defect lists.

---

## 1. Team information

| Item | Detail |
|------|--------|
| **Team** | `STQA Group 10` |
| **Class** | `<!-- e.g. SE001.P11 -->` |
| **Report date** | `07/06/2026` |
| **Tested system** | https://stqa.rbc.vn — v1.0 |

---

## 2. Overall results

| Metric | Value |
|--------|-------|
| Total test cases | `47` |
| Pass | `21` |
| Fail | `11` |
| Blocked | `0` |
| Not Run | `15` |
| **Pass rate** | `44.68%` |
| **Bugs found** | `7` |

### Distribution by functional area

| Functional area | # TC | Pass | Fail | Blocked | Notes |
|-----------------|-----:|------|------|---------|-------|
| Login | 9 | 5 | 4 | 0 | Pass rate: 55.56% |
| View Book List | 7 | 0 | 0 | 7 | Pass rate: 0% (not executed) |
| Search & Filter | 8 | 0 | 0 | 8 | Pass rate: 0% (not executed) |
| Borrow Book | 6 | 4 | 2 | 0 | Pass rate: 66.67% |
| Return Book | 6 | 5 | 1 | 0 | Pass rate: 83.33% |
| Overdue Handling | 2 | 2 | 0 | 0 | Pass rate: 100% |
| Manage Members (Add New) | 7 | 3 | 4 | 0 | Pass rate: 42.86% |
| Borrow Receipt Lookup | 2 | 2 | 0 | 0 | Pass rate: 100% |

### Bugs by severity

| Severity | Count | Bug IDs |
|---------|------:|---------|
| High | | |
| Medium | | |
| Low | | |

**Bugs found (IDs)**: BUG-01, BUG-02, BUG-03, BUG-04, BUG-08, BUG-09, BUG-10 (7 total)

---

## 3. Design techniques applied (IDM / test design)

| Technique | Applied to REQs | # of TCs (approx) | Notes |
|-----------|------------------|------------------:|-------|
| Equivalence Partitioning (EP) | REQ-01, REQ-02, REQ-03, REQ-04, REQ-07, REQ-08 | ~40 | Widely used for positive/negative input partitions |
| Boundary Value Analysis (BVA) | REQ-01, REQ-02, REQ-03, REQ-04 | ~10 | Applied to numeric/date and input length boundaries |
| Decision Table | REQ-01, REQ-02, REQ-03, REQ-04, REQ-07 | ~12 | Used for combinations of business rules and roles |
| State Transition Testing (STT) | REQ-06 | 2 | Used for overdue state transitions (Borrowed → Overdue) |
| Black-box Testing | REQ-01, REQ-07 | ~9 | Exploratory / behavior validation |

---

## 4. Quality analysis

### 4.1 Strengths
`<!-- list functional areas that work well (observed from test cases) -->`

### 4.2 Weaknesses
`<!-- list critical issues or frequent failures -->`

---

## 5. Priority recommendations for fixes

> Clarify prioritization criteria: use severity (technical impact) and/or business priority.

| Priority | Bug | Severity | Rationale |
|---------:|-----|---------:|-----------|
| | | | |

---

## 6. Conclusion

`<!-- Overall assessment: Is the system ready for release? Why / why not? -->`

---

## 7. Lessons learned (optional)

`<!-- What the team learned from testing -->`

---

## 8. AI usage declaration (optional)

| AI tool | Used for | How reviewed/edited |
|--------|----------|--------------------|
| | | |
