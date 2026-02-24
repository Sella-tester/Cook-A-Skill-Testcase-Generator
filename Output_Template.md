# Output Template — Smart Test Case Generator

> This template shows the exact output structure the skill produces. Use it as a reference for expected format.

---

## Phase 1 — Pre-Analysis Report

```
📋 Pre-Analysis Report: [Feature Name]
────────────────────────────────────────
ACs Identified:        [N]
Logic Gaps Found:      [N]
Security Risks Found:  [N]
Recommended Techniques: [Equivalence Partitioning, BVA, State Transition, ...]

Estimated Test Cases:
  ✅ Happy Path:   ~[N]
  ⚠️ Edge Case:    ~[N]
  ❌ Negative:     ~[N]
  🔒 Security:     ~[N]    ← shown only when triggers detected
  🖥️ UI/UX:        ~[N]    ← shown only when triggers detected
  📱 Mobile:       ~[N]    ← shown only when triggers detected
  🔌 API:          ~[N]    ← shown only when triggers detected
  ─────────────────────
  📊 Total:        ~[N]

Priority Breakdown:
  🔴 Critical: ~[N]
  🟡 Major:    ~[N]
  🟢 Minor:    ~[N]

⚠️ Logic Gaps Detected:
  1. [Description of gap — e.g., "Spec does not define behavior when email already exists"]
  2. [Description of gap]

🔒 Security Risks Detected:
  1. [Description — e.g., "Login form detected — auth bypass and brute force cases recommended"]
  2. [Description]

Shall I proceed with test case generation? [Yes / No / Modify scope]
```

---

## Phase 2 — Test Case Table

### Standard Format (Mentor Fields ON — Default)

| ID | AC Ref | Title | Type | Pre-condition | Test Data | Steps | Expected Result | Priority | Design Logic | Execution Pro-tip |
|----|--------|-------|------|---------------|-----------|-------|-----------------|----------|-------------|-------------------|
| TC-LOGIN-001 | AC1 | Login with valid email and password | ✅ Happy Path | User account exists and is active | email: `user@example.com`, password: `Str0ng!Pass1` | 1. Navigate to /login 2. Enter email 3. Enter password 4. Click "Login" | User is redirected to dashboard. Welcome message displays: "Hello, [username]" | 🔴 Critical | **EP** — Valid partition for email + password combination | Verify redirect URL is `/dashboard`, not just any page. Check welcome message includes actual username. |
| TC-LOGIN-002 | AC1 | Login with unregistered email | ❌ Negative | No account with this email exists | email: `unknown@example.com`, password: `Str0ng!Pass1` | 1. Navigate to /login 2. Enter unregistered email 3. Enter password 4. Click "Login" | Error message: "Invalid email or password" (generic — does not reveal which field is wrong) | 🔴 Critical | **Security best practice** — Generic error prevents email enumeration | Confirm error message is identical for wrong email vs wrong password (no information leakage). |
| TC-LOGIN-003 | AC2 | Login with password at minimum length boundary | ⚠️ Edge Case | User account exists with 8-char password | email: `user@example.com`, password: `Ab1!xxxx` (8 chars) | 1. Navigate to /login 2. Enter email 3. Enter 8-character password 4. Click "Login" | Login succeeds. User redirected to dashboard. | 🟡 Major | **BVA** — Testing exact minimum boundary (8 chars) | Also test with 7 chars (below boundary) to confirm rejection. |

### Senior Mode (Mentor Fields OFF)

| ID | AC Ref | Title | Type | Pre-condition | Test Data | Steps | Expected Result | Priority |
|----|--------|-------|------|---------------|-----------|-------|-----------------|----------|
| TC-LOGIN-001 | AC1 | Login with valid email and password | ✅ Happy Path | User account exists and is active | email: `user@example.com`, password: `Str0ng!Pass1` | 1. Navigate to /login 2. Enter email 3. Enter password 4. Click "Login" | User is redirected to dashboard. Welcome message: "Hello, [username]" | 🔴 Critical |

---

## Phase 3 — Security Cases (Auto-triggered)

| ID | AC Ref | Title | Type | Pre-condition | Test Data | Steps | Expected Result | Priority |
|----|--------|-------|------|---------------|-----------|-------|-----------------|----------|
| TC-LOGIN-010 | SEC | SQL injection in email field | 🔒 Security | Login page accessible | email: `' OR 1=1--`, password: `any` | 1. Navigate to /login 2. Enter SQL injection payload in email 3. Enter any password 4. Click "Login" | Login fails. No SQL error exposed. Error message: "Invalid email or password" | 🔴 Critical |
| TC-LOGIN-011 | SEC | XSS in email field | 🔒 Security | Login page accessible | email: `<script>alert('xss')</script>`, password: `any` | 1. Navigate to /login 2. Enter XSS payload in email 3. Click "Login" | Input is sanitized. No script execution. Error message displayed normally. | 🔴 Critical |
| TC-LOGIN-012 | SEC | Brute force — rapid login attempts | 🔒 Security | Login page accessible | email: `user@example.com`, password: `wrong` × 10 rapid attempts | 1. Navigate to /login 2. Submit 10 rapid login attempts with wrong password | Account locked or rate limited after [TBD] attempts. `# REVIEW: lockout threshold not in spec` | 🔴 Critical |

---

## Phase 4 — Report Template

### Test Execution Table

| TC ID | AC Ref | Title | Type | Priority | Status | Actual Result | Bug ID | Notes |
|-------|--------|-------|------|----------|--------|---------------|--------|-------|
| TC-LOGIN-001 | AC1 | Login with valid email and password | ✅ Happy Path | 🔴 | ☐ Pass / ☐ Fail / ☐ Blocked | | | |
| TC-LOGIN-002 | AC1 | Login with unregistered email | ❌ Negative | 🔴 | ☐ Pass / ☐ Fail / ☐ Blocked | | | |
| TC-LOGIN-003 | AC2 | Login with password at min length | ⚠️ Edge | 🟡 | ☐ Pass / ☐ Fail / ☐ Blocked | | | |

### Coverage Summary

| Test Type | Count | Percentage |
|-----------|-------|------------|
| ✅ Happy Path | [N] | [%] |
| ⚠️ Edge Case | [N] | [%] |
| ❌ Negative | [N] | [%] |
| 🔒 Security | [N] | [%] |
| **Total** | **[N]** | **100%** |

### Test Summary

| Metric | Value |
|--------|-------|
| Total Test Cases | [N] |
| Passed | [N] |
| Failed | [N] |
| Blocked | [N] |
| **Pass Rate** | **[%]** |
| Critical Cases Passed | [N]/[N] |

### Bugs Found

| Bug ID | TC ID | Type | Severity | Description |
|--------|-------|------|----------|-------------|
| BUG-001 | TC-LOGIN-00X | [Type] | 🔴 Critical / 🟡 Major / 🟢 Minor | [Description] |

### Sign-off

| Field | Value |
|-------|-------|
| QC Name | _________________ |
| Date | _________________ |
| Notes | _________________ |

### Risk Notes

```
⚠️ Risk Notes
──────────────
🔲 TBDs (values not in spec):
  - TC-LOGIN-012: Lockout threshold not specified → [TBD]

🔲 Logic Gaps:
  - "Remember me" behavior not defined in spec
  - Max login attempts before lockout not specified

🔲 Test Data to Prepare:
  - Active user account with known credentials
  - Locked/suspended account for negative tests

🔲 Hallucination Flags:
  - None detected ✅ (or list flagged items)
```

---

## AC Coverage Matrix

| AC / BR | Description | TC IDs Covering | Status |
|---------|-------------|----------------|--------|
| AC1 | User can login with valid email and password | TC-LOGIN-001, TC-LOGIN-002 | ✅ Covered |
| AC2 | Password must be 8–64 characters | TC-LOGIN-003, TC-LOGIN-004 | ✅ Covered |
| BR1 | Account locks after N failed attempts | TC-LOGIN-012 | ⚠️ Flagged — threshold [TBD], confirm with PO |
| *(Logic Gap)* | "Remember me" checkbox behavior | — | ⚠️ Flagged — not in spec, confirm with PO |

---

## Review Prompt

```
✅ Generation complete!

Test Cases: [N] total
  🔴 Critical: [N] | 🟡 Major: [N] | 🟢 Minor: [N]
Coverage: [N]/[N] ACs covered (100%)
Flags: [N] TBDs | [N] Logic Gaps

What would you like to do?
(1) Approve all and export
(2) Edit a test case — provide the TC ID and what to change
(3) Reject a test case — provide the TC ID and reason
(4) Add more cases (edge / security / specific AC)
(5) Change export format (Markdown / JSON / CSV)
```
