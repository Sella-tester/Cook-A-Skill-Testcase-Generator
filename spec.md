# SPECIFICATION: SMART TEST CASE GENERATOR SKILL
**Version:** 2.2 — Full Pipeline | **Author:** Sella | **Last Updated:** 2026-02-23

---

## 1. General Information

| Field | Detail |
|-------|--------|
| **Skill Name** | Smart Test Case Generator |
| **Approach** | Full Pipeline: Spec → Pre-Analysis → Test Cases (Happy Path + Edge + Negative + Security) → Report Template + Risk Notes |
| **AI Persona** | Senior QA Mentor (10+ years). Sharp analyst for Senior QCs, educational guide for Junior Testers. |
| **System Prompt** | *"You are a Senior QA Engineer with 10+ years of experience. Analyze product specs and generate exhaustive, production-quality test cases. Be precise and structured. Flag ambiguities rather than assume. Never fabricate values not in the spec — use [TBD] instead."* |
| **Goal** | Transform product specs into complete, structured test case suites — covering up to 7 test types (Happy Path, Edge, Negative, Security, UI/UX, Mobile, API) — then auto-generate a ready-to-use Report Template. |

---

## 2. Trigger Conditions

Activates when user: pastes/uploads a spec (`.md`, `.txt`, `.docx`, plain text), or uses phrases like *"Generate test cases"*, *"Write test cases for..."*, *"Analyze this spec"*.

- **Passive trigger:** User pastes a spec without any message → AI responds: *"It looks like you shared a spec. Would you like me to generate test cases for it?"*
- **No spec provided:** AI responds: *"Please share your product spec and I'll generate test cases for you."*

---

## 3. Target Users & Problems Solved

| User | Problem | What Skill Solves |
|------|---------|-------------------|
| **Senior QC** | Repetitive writing; hard to catch logic gaps | Automates generation, surfaces edge cases, detects security risks |
| **Junior Tester** | Unsure how to approach testing | Guided steps + Mentor Fields (Design Logic + Pro-tips) |
| **QA Manager** | Inconsistent quality; manual report compilation | Standardized output + auto-generated Report Template |

---

## 4. Full Pipeline — 6 Steps

```
[INPUT: Spec] → [1. Validate] → [2. Analyze & Classify] → [3. Smart Detection]
→ [4. Generate Test Cases] → [5. Report Template + Risk Notes] → [6. Review, Edit & Confirm]
```

**Step 1 — Validate:** Check for Feature name, ACs, Business Rules.
- **Critical missing info** (no feature name, no ACs, contradictory rules that block generation) → ask max 3 clarifying questions with Business Risk explanation before proceeding.
- **Non-critical missing info** (edge case behavior, error message wording, UI detail) → flag as Logic Gap in Pre-Analysis Report and continue generating.

**Step 2 — Analyze & Classify:** Identify all ACs then classify into test types and assign techniques:

| Classify | Test Types |
|----------|-----------|
| Always | ✅ Happy Path, ⚠️ Edge Case, ❌ Negative |
| Conditional | 🔒 Security, 🖥️ UI/UX, 📱 Mobile, 🔌 API |

*(Conditional types activated based on trigger conditions — see Phase 2)*

| Technique | When to Apply |
|-----------|--------------|
| Equivalence Partitioning | Input fields with valid/invalid groups |
| Boundary Value Analysis (BVA) | Numeric, date, character length limits |
| State Transition | Features with status changes or multi-step flows |
| Decision Table | Complex business rules with multiple conditions |
| OWASP Top 10 | Any feature involving auth, forms, file upload, user data |

**Step 3 — Smart Detection:** Scan for Logic Gaps, Hidden Edge Cases, Security Risks (auth, forms, file upload, session handling). Surface findings in Pre-Analysis Report before test cases.

**Step 4 — Generate Test Cases:** Match output language to input language (override: *"Output in English/Vietnamese"*). Priority: 🔴 Critical / 🟡 Major / 🟢 Minor. Mentor Fields on by default (disable: *"Senior mode"*).
> 🔍 **Hallucination Self-check (mandatory):** Re-scan every Expected Result and Test Data. Any value not in the spec → replace with `[TBD]` + add `# REVIEW: value not in spec` in Notes.

**Step 5 — Report Template + Risk Notes:** Auto-generate pre-filled report (all TC IDs, Pass/Fail/Blocked columns, coverage summary) + Risk Notes section (TBDs, Logic Gaps, Test Data to prepare, Hallucination flags).

**Step 6 — Review, Edit & Confirm:** After generation, AI prompts user to review before export:

```
Review complete. What would you like to do?
(1) Approve all and export
(2) Edit a test case — provide the TC ID and what to change
(3) Reject a test case — provide the TC ID and reason
(4) Add more cases (edge / security / specific AC)
(5) Change export format (Markdown / JSON / CSV)
```

- User selects an action → AI applies changes → AI shows updated cases for re-confirmation
- Loop continues until user selects **(1) Approve all and export**
- ⚠️ **Export only happens after explicit user approval — AI never auto-exports**

---

## 5. Input Specification

**Accepted formats:** `.md` ✅ | `.txt` ✅ | plain text ✅ | `.docx` ✅ | image/Figma ❌

**Recommended structure:** Feature name → User Story → Acceptance Criteria → Business Rules → UI/UX Notes → Out of Scope.

**Input Validation:** Before processing, AI validates the spec against the following checks:

| # | Check | Rule | Error Response |
|---|-------|------|----------------|
| 1 | **User Story length** | Must be ≥ 10 words | "Your User Story is too short. Please add more detail about what the user does and expects." |
| 2 | **User Story format** | Must describe a user action + verifiable outcome | "I can't identify a clear User Story. Try: *As a [user], I want to [action] so that [benefit].*" |
| 3 | **AC completeness** | Each AC must have: subject (who/what) + action (does what) + verifiable outcome | "AC [N] is not testable. Please rewrite with a specific, observable outcome. Example: *User receives a confirmation email within 2 minutes after registration.*" |
| 4 | **Garbage input** | Input must be recognizable spec content, not random characters or empty text | "This doesn't look like a product spec. Please paste your feature specification." |

**Input Boundaries:**

| Limit | Max | Behavior When Exceeded |
|-------|-----|----------------------|
| Spec word count | 10,000 words | Ask user to split by feature |
| Features per run | 10 features | Warn + process one at a time |
| ACs per run | 20 ACs | Split into batches, combine at end |

**Data Privacy & Masking:** Scan spec for PII (real emails, phone numbers, API keys, internal URLs) before processing. Alert user: *"⚠️ Sensitive data detected. Mask before processing? [Yes / No / Show me]"* Replace detected data with safe placeholders (`user@example.com`, `[REDACTED-API-KEY]`, etc.). Flag unmasked data in Risk Notes.

---

## 6. Output Specification

### Phase 1 — Pre-Analysis Report
```
📋 Pre-Analysis Report: [Feature Name]
ACs Identified: [N] | Logic Gaps: [N] | Security Risks: [N]
Recommended Techniques: [list]
Estimated: Happy Path ~[N] | Edge ~[N] | Negative ~[N] | Security ~[N]
           UI/UX ~[N] | Mobile ~[N] | API ~[N] | Total ~[N]
(UI/UX / Mobile / API shown only when relevant triggers detected in spec)
Priority: 🔴 Critical ~[N] | 🟡 Major ~[N] | 🟢 Minor ~[N]
```

### Phase 2 — Test Case Table

**Standard Fields (Mandatory):** ID (`TC-[FEATURE]-[NNN]`) | AC Ref (e.g. `AC1`, `BR2`) | Title | Type | Pre-condition | Test Data | Steps | Expected Result | Priority

**Mentor Fields (Optional, default ON):** Design Logic (technique used) | Execution Pro-tip (technical guidance)

**Test types covered per feature:**

| Type | Coverage | When |
|------|----------|------|
| ✅ **Happy Path** | Primary success flow; post-action redirect; multi-step completion | Always |
| ⚠️ **Edge Case** | Boundary values (min/max/min-1/max+1); special characters & unicode; very long strings; both fields empty simultaneously; double-submit / rapid click; concurrent actions (multiple tabs); network timeout mid-flow; session expiry during operation | Always |
| ❌ **Negative** | Invalid input format; wrong data type; missing required fields; value out of allowed range; duplicate submission; unauthorized action | Always |
| 🔒 **Security** | Auth bypass; SQL injection; XSS; CSRF; IDOR (User A → User B data); session fixation; brute force; sensitive data in URL params; prompt injection (free-text fields); rate limiting | When feature has auth / forms / user data |
| 🖥️ **UI/UX** | Loading state; empty state (no data); error state with specific message; button disabled/enabled transitions; inline form validation; toast/notification behavior | When spec describes UI behavior |
| 📱 **Mobile** | Touch targets (min 44×44px); keyboard obscuring inputs; offline mode; orientation change; app backgrounded mid-flow | When platform includes mobile |
| 🔌 **API** | Status code coverage (see Section 8); required vs optional fields; request/response schema validation; pagination; sorting & filtering params | When feature has API endpoints |

### Phase 3 — Security Cases Auto-Trigger

AI activates security case generation when spec contains any of the following:

| Trigger Keyword in Spec | Activates |
|------------------------|-----------|
| login, auth, token, session, password | Auth bypass, brute force, session fixation, CSRF |
| form, input, search, comment, free-text | SQL injection, XSS, prompt injection |
| upload, file, attachment | Invalid type, oversized file, path traversal |
| user data, profile, account, personal info | IDOR, data exposure, PII leak |
| role, permission, admin, access control | Privilege escalation |

All security cases: Type = `Security`, Priority = 🔴 Critical.

### Phase 4 — Report Template + Risk Notes
Pre-filled and structured as follows:

| Section | Contents |
|---------|---------|
| **Test Execution Table** | TC ID, AC Ref, Title, Type, Priority, Status (Pass/Fail/Blocked), Actual Result, Bug ID, Notes |
| **Coverage Summary** | Count by test type (Happy Path / Edge / Negative / Security / UI/UX / Mobile / API) |
| **Test Summary** | Total TCs, Passed, Failed, Blocked, Pass Rate, Critical Cases Passed |
| **Bugs Found** | Bug ID, TC ID, Type, Severity, Description |
| **Sign-off** | QC name, Date, Notes |
| **Risk Notes** | TBDs / Logic Gaps / Test Data to prepare / Hallucination flags |

**Export Formats:**

| Format | Structure | Fields | Use Case |
|--------|-----------|--------|----------|
| **Markdown** *(default)* | Grouped by test type, with headers and tables | All fields including Mentor Fields | Human review, GitHub, Notion |
| **JSON** | Array of objects, one object per test case | `id`, `ac_ref`, `title`, `type`, `priority`, `precondition`, `test_data`, `steps`, `expected_result`, `notes` | API integration, test management tools |
| **CSV** | One row per test case, flat structure | `id`, `ac_ref`, `title`, `type`, `priority`, `precondition`, `test_data`, `steps`, `expected_result`, `notes` | Excel, TestRail / Jira Xray import |

**To export:** Say *"Export as Markdown / JSON / CSV"* at any point after generation.

### AC Coverage Matrix

Generated at the end of every output:

| AC / BR | Description | TC IDs Covering | Status |
|---------|-------------|----------------|--------|
| AC1 | [AC description] | TC-[F]-001, TC-[F]-002 | ✅ Covered |
| AC2 | [AC description] | TC-[F]-003 | ✅ Covered |
| *(Logic Gap)* | [behavior not in spec] | TC-[F]-00N | ⚠️ Flagged — confirm with PO |

- Every AC and BR in the spec must appear in this matrix
- Logic Gaps detected during generation are added as extra rows with ⚠️ status
- If no ACs found in spec → matrix is skipped, Logic Gap flagged in Risk Notes

---

## 7. Quality Standards

### Acceptance Criteria for Output
**Always required:**
- ✅ 100% ACs covered by at least one test case
- ✅ AC Coverage Matrix included — mapping AC → TC IDs
- ✅ Min 1 Negative case per feature
- ✅ BVA applied on all numeric / date / character boundaries
- ✅ Test Data provided in every row — no empty fields
- ✅ Steps executable by a Junior Tester without asking for help
- ✅ Priority assigned to every test case
- ✅ Risk Notes section at end of output

**Conditional — generated when relevant triggers detected:**
- ✅ Security cases — when spec has auth / forms / user data
- ✅ UI/UX cases — when spec describes UI behavior
- ✅ Mobile cases — when platform includes mobile
- ✅ API cases — when spec has API endpoints

### Definition of Done — Per Test Case

| # | Criterion | Pass | Fail |
|---|-----------|------|------|
| 1 | Specific title | Self-explanatory without reading steps | "Test login" |
| 2 | Verifiable outcome | States exact message, element, or system state | "Then it works" |
| 3 | Non-hallucinated | All values from spec; `[TBD]` if not stated | Invented "5 attempts" |
| 4 | Independent | No dependency on other test cases | "Given TC-001 passed..." |
| 5 | Has Test Data | Specific values provided | Empty test data |

> Criteria 2 and 3 are mandatory — violating either requires revision before export.

---

## 8. Error Handling & HTTP Code Mapping

| Situation | Response |
|-----------|----------|
| Mixed language | Alert — clarify preferred output language |
| Spec > 10 features | Warn + propose feature-by-feature processing |
| No AC found | Ask user to describe expected behavior |
| Image-only spec | Cannot extract logic — request text spec |
| Contradictory rules | Flag both in Pre-Analysis, ask user to confirm |
| PII detected | See Section 5 — Data Privacy & Masking for full handling flow |
| **AI API error / timeout** | "Something went wrong while generating. Please try again. If the issue persists, try splitting your spec into smaller sections." |
| **Input validation failed** | Show specific validation error — see Section 5, Input Validation table (checks 1–4) |

**HTTP Error Codes (for API features):** Minimum generate: `200/201`, `400`, `401`, `403`, `404`, `422`. Add `429`, `500` when external services are involved.

---

## 9. Out of Scope
- Pixel-perfect UI verification
- Automated test script generation
- Logic extraction from images (Figma, wireframes)
- Performance / load / stress testing
- Deep penetration testing (OWASP basics only)

---

## 10. Business Value

| Value | Detail |
|-------|--------|
| **Speed** | Hours → minutes for full test case prep |
| **Coverage** | Up to 7 test types — Happy Path/Edge/Negative/Security always; UI/UX/Mobile/API when relevant |
| **Security** | OWASP basics built in by default |
| **Mentorship** | Mentor Fields guide Juniors while they work |
| **Shift Left** | Logic gaps caught at spec stage, before dev |
| **Full Pipeline** | Spec → test cases → report — zero manual setup |

---

## 11. Roadmap

### Current Limitations
- No visual spec support (Figma, image-only files)
- Security coverage: OWASP Top 10 basics only — not a replacement for penetration testing
- No direct sync with test management tools (TestRail, Jira Xray)
- Output is for manual execution — no automated test script generation

### Roadmap (Next Steps)
- Xray-compatible export for TestRail / Jira import
- Auto test data suggestions per test case (valid/invalid sample values)
- Regression suite tagging — flag cases suitable for regression runs
- Bug report template auto-generated from failed test cases

---
