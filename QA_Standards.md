# QA Standards Reference — Smart Test Case Generator

> Reference document for testing techniques, naming conventions, and quality criteria used by this skill.

---

## 1. Testing Techniques

### Equivalence Partitioning (EP)
Divide input data into groups (partitions) where all values in a group are expected to behave the same way.

| Partition Type | Example (Age field: 18–65) |
|---------------|---------------------------|
| Valid | 25, 40, 60 |
| Invalid (below) | -1, 0, 17 |
| Invalid (above) | 66, 100, 999 |
| Invalid (type) | "abc", null, empty |

**When to use:** Any input field with defined valid/invalid ranges.

### Boundary Value Analysis (BVA)
Test at exact boundaries: min, min+1, max-1, max, min-1, max+1.

| Boundary | Values to Test (Range: 1–100) |
|----------|------------------------------|
| Below minimum | 0 |
| Minimum | 1 |
| Just above minimum | 2 |
| Just below maximum | 99 |
| Maximum | 100 |
| Above maximum | 101 |

**When to use:** Numeric fields, character length limits, date ranges, pagination.

### State Transition Testing
Model features with distinct states and transitions between them.

```
Example: Order Status
[Created] → [Paid] → [Shipped] → [Delivered]
                ↘ [Cancelled]
         [Paid] → [Refunded]
```

**Test:** Valid transitions, invalid transitions (e.g., Created → Delivered), and terminal states.

**When to use:** Features with status flows, multi-step wizards, approval workflows.

### Decision Table Testing
Map combinations of conditions to expected outcomes.

| Condition 1 | Condition 2 | Condition 3 | Expected Result |
|-------------|-------------|-------------|-----------------|
| True | True | True | Action A |
| True | True | False | Action B |
| True | False | — | Action C |
| False | — | — | Action D |

**When to use:** Complex business rules with multiple conditions (discount rules, access control, pricing tiers).

### OWASP Top 10 Basics
Security testing based on the most common web application vulnerabilities.

| Vulnerability | What to Test |
|--------------|-------------|
| **Injection (SQL/XSS)** | Special characters in input fields: `' OR 1=1--`, `<script>alert(1)</script>` |
| **Broken Auth** | Default credentials, session token predictability, brute force |
| **Sensitive Data Exposure** | PII in URLs, unencrypted storage, verbose error messages |
| **Broken Access Control (IDOR)** | Change user ID in URL/request to access another user's data |
| **Security Misconfiguration** | Default settings, unnecessary HTTP methods, stack traces |
| **CSRF** | State-changing actions without CSRF tokens |
| **SSRF** | Internal URL access via user-supplied URLs |

**When to use:** Any feature involving authentication, user input, file upload, or personal data.

---

## 2. Test Case Naming Convention

### ID Format
```
TC-[FEATURE_ABBREVIATION]-[NNN]
```

| Component | Rule | Example |
|-----------|------|---------|
| `TC` | Fixed prefix | `TC` |
| `FEATURE` | 2–5 character abbreviation of feature name | `LOGIN`, `REG`, `CART`, `PROF` |
| `NNN` | Sequential 3-digit number | `001`, `002`, `015` |

**Examples:**
- `TC-LOGIN-001` — Login with valid credentials
- `TC-REG-005` — Registration with password below minimum length
- `TC-CART-012` — Add item when cart is full

### Title Rules
- Must be self-explanatory without reading steps
- Format: `[Action] + [condition/context] + [expected outcome hint]`
- ✅ *"Login with valid email and password succeeds"*
- ❌ *"Test login"*
- ❌ *"TC for AC1"*

---

## 3. Priority Classification

| Priority | Symbol | Criteria | Examples |
|----------|--------|----------|---------|
| **Critical** | 🔴 | Core functionality, security, data integrity | Login flow, payment processing, auth bypass |
| **Major** | 🟡 | Important but not blocking, significant UX impact | Form validation, error messages, edge cases |
| **Minor** | 🟢 | Cosmetic, nice-to-have, low-frequency scenarios | Tooltip text, alignment, rare edge cases |

### Assignment Rules
- All **Happy Path** cases for core features → 🔴 Critical
- All **Security** cases → 🔴 Critical
- **Edge Cases** on critical fields → 🟡 Major
- **Edge Cases** on non-critical fields → 🟢 Minor
- **Negative** cases on required fields → 🟡 Major
- **UI/UX** cases → 🟡 Major or 🟢 Minor (context-dependent)

---

## 4. Test Data Standards

### Rules
1. **Never leave Test Data empty** — every test case must have specific values
2. **Use realistic values** — not "test123" or "asdf"
3. **Use spec values when available** — if the spec says "max 50 characters", use exactly 50, 51, and 49
4. **Use `[TBD]` for unknown values** — never fabricate data not in the spec
5. **Provide both valid and invalid examples** where applicable

### Sample Test Data by Type

| Data Type | Valid Examples | Invalid Examples |
|-----------|--------------|-----------------|
| Email | `user@example.com`, `test.name@domain.co` | `user@`, `@domain.com`, `user space@domain.com` |
| Password | `Str0ng!Pass`, `MyP@ssw0rd123` | `123`, `password`, `aaa` |
| Phone | `+84901234567`, `0901234567` | `abc`, `12345`, `+00000000000` |
| Name | `Nguyễn Văn A`, `John Doe` | `""`, `<script>`, 256 chars |
| Number (1-100) | `1`, `50`, `100` | `0`, `101`, `-5`, `abc` |
| Date | `2026-01-15`, `2026-12-31` | `2026-13-01`, `0000-00-00`, `abc` |
| URL | `https://example.com` | `not-a-url`, `javascript:alert(1)` |
| File upload | `document.pdf` (5MB) | `script.exe`, `file.pdf` (101MB) |

---

## 5. AC Coverage Requirements

| Requirement | Mandatory? |
|------------|------------|
| Every AC mapped to at least 1 test case | ✅ Yes |
| Every BR (Business Rule) mapped to at least 1 test case | ✅ Yes |
| AC Coverage Matrix included in output | ✅ Yes |
| Logic Gaps flagged with ⚠️ status | ✅ Yes |
| Uncovered ACs reported in Risk Notes | ✅ Yes |

---

## 6. Definition of Done — Per Test Case

| # | Criterion | ✅ Pass Example | ❌ Fail Example |
|---|-----------|----------------|----------------|
| 1 | **Specific title** | "Register with password shorter than 8 characters shows error" | "Test registration" |
| 2 | **Verifiable outcome** | "Error message: 'Password must be at least 8 characters'" | "Then it shows error" |
| 3 | **Non-hallucinated** | All values from spec; `[TBD]` if not stated | Invented "max 5 attempts" |
| 4 | **Independent** | Can run without other test cases | "Given TC-001 passed first..." |
| 5 | **Has Test Data** | `email: user@example.com, password: Ab1!` | (empty) |

> **Criteria 2 and 3 are mandatory** — violating either blocks export until fixed.

---

## 7. Report Template Structure

| Section | Purpose |
|---------|---------|
| **Test Execution Table** | Track Pass/Fail/Blocked per TC |
| **Coverage Summary** | Count by test type |
| **Test Summary** | Total, Passed, Failed, Blocked, Pass Rate |
| **Bugs Found** | Link bugs to TC IDs |
| **Sign-off** | QC name, date, approval |
| **Risk Notes** | TBDs, Logic Gaps, test data prep, hallucination flags |
