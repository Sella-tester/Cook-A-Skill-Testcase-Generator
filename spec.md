# SPECIFICATION: SMART TEST CASE GENERATOR SKILL
**Version:** 2.1 — Full Pipeline | **Author:** Sella | **Last Updated:** 2026-02-23

---

## 1. General Information

| Field | Detail |
|-------|--------|
| **Skill Name** | Smart Test Case Generator |
| **Approach** | Full Pipeline: Spec → Pre-Analysis → Test Cases → Security Cases → Report Template + Risk Notes |
| **AI Persona** | Senior QA Mentor (10+ years). Sharp analyst for Senior QCs, educational guide for Junior Testers. |
| **Goal** | Transform product specs into complete, structured test case suites — covering all 4 types — then auto-generate a ready-to-use Report Template. |

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
→ [4. Generate Test Cases] → [5. Report Template + Risk Notes] → [6. Refine Loop]
```

**Step 1 — Validate:** Check for Feature name, ACs, Business Rules. If ambiguous → ask max 3 clarifying questions with Business Risk explanation. Do NOT stop generating — flag gaps and continue.

**Step 2 — Analyze & Classify:** Identify all ACs. Classify by type: ✅ Happy Path | ⚠️ Edge Case | ❌ Negative | 🔒 Security. Apply techniques: Equivalence Partitioning, BVA, State Transition, Decision Table, OWASP Top 10.

**Step 3 — Smart Detection:** Scan for Logic Gaps, Hidden Edge Cases, Security Risks (auth, forms, file upload, session handling). Surface findings in Pre-Analysis Report before test cases.

**Step 4 — Generate Test Cases:** Match output language to input language (override: *"Output in English/Vietnamese"*). Priority: 🔴 Critical / 🟡 Major / 🟢 Minor. Mentor Fields on by default (disable: *"Senior mode"*).
**Hallucination Self-check (mandatory):** Re-scan every Expected Result and Test Data. Any value not in the spec → replace with `[TBD]` + add `# REVIEW: value not in spec` in Notes.

**Step 5 — Report Template + Risk Notes:** Auto-generate pre-filled report (all TC IDs, Pass/Fail/Blocked columns, coverage summary) + Risk Notes section (TBDs, Logic Gaps, Test Data to prepare, Hallucination flags).

**Step 6 — Refine Loop:** After output, offer: add edge cases / expand steps / add security cases / adjust priority / process next feature.

---

## 5. Input Specification

**Accepted formats:** `.md` ✅ | `.txt` ✅ | plain text ✅ | `.docx` ✅ | image/Figma ❌

**Recommended structure:** Feature name → User Story → Acceptance Criteria → Business Rules → UI/UX Notes → Out of Scope.

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
Estimated: Happy Path ~[N] | Edge ~[N] | Negative ~[N] | Security ~[N] | Total ~[N]
Priority: 🔴 Critical ~[N] | 🟡 Major ~[N] | 🟢 Minor ~[N]
```

### Phase 2 — Test Case Table

**Standard Fields (Mandatory):** ID (`TC-[FEATURE]-[NNN]`) | Title | Type | Pre-condition | Test Data | Steps | Expected Result | Priority

**Mentor Fields (Optional, default ON):** Design Logic (technique used) | Execution Pro-tip (technical guidance)

**Test types covered per feature:**
- ✅ **Happy Path** — primary success flow (always)
- ⚠️ **Edge Case** — boundary values, unusual valid inputs (always)
- ❌ **Negative** — invalid inputs, empty fields, wrong format (always)
- 🔒 **Security** — auth bypass, SQL injection, XSS, session, brute force (when feature has auth/forms/data)

### Phase 3 — Security Cases Auto-Trigger

| Feature Type | Security Cases Generated |
|-------------|--------------------------|
| Auth / Login | Auth bypass, brute force, session fixation |
| Input fields | SQL Injection, XSS (basic) |
| File upload | Invalid type, oversized, path traversal |
| User data | IDOR — User A accessing User B's data |
| Role-based | Privilege escalation |

All security cases: Type = `Security`, Priority = 🔴 Critical.

### Phase 4 — Report Template + Risk Notes
Pre-filled with all TC IDs, Type, Priority, Pass/Fail/Blocked columns, Coverage Summary by type, Test Summary metrics, Bugs Found table, Sign-off, and Risk Notes (TBDs / Logic Gaps / Test Data needed / Hallucination flags).

### AC Coverage Matrix
Table mapping each AC → TC IDs that cover it. Flags Logic Gaps pending PO confirmation.

---

## 7. Quality Standards

### Acceptance Criteria for Output
✅ 100% ACs covered | ✅ AC Coverage Matrix included | ✅ Min 1 Negative per feature | ✅ BVA applied on all boundaries | ✅ Security cases for auth/forms/data | ✅ Test Data in every row | ✅ Steps executable by Junior without help | ✅ Priority on every case | ✅ Risk Notes section at end of output

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
| PII detected | Alert + offer masking before processing |

**HTTP Error Codes (for API features):** Minimum generate: `200/201`, `400`, `401`, `422`. Add `429`, `500` when external services are involved.

---

## 9. Out of Scope & Limitations
- Visual/pixel-perfect UI verification
- Automated test script generation
- Logic extraction from images (Figma, wireframes)
- Performance / load / stress testing
- Deep penetration testing (OWASP basics only)

---

## 10. Business Value

| Value | Detail |
|-------|--------|
| **Speed** | Hours → minutes for full test case prep |
| **Coverage** | 4 test types every time — no cases missed |
| **Security** | OWASP basics built in by default |
| **Mentorship** | Mentor Fields teach Juniors while they work |
| **Shift Left** | Logic gaps caught at spec stage, before dev |
| **Full Pipeline** | Spec → test cases → report — zero manual setup |

---

## 11. Roadmap

**Current Limitations:** No visual spec support | OWASP basics only | No TestRail/Jira sync | Manual execution only

**Next Steps:** CSV/Xray export for TestRail/Jira | Auto test data suggestions | Regression suite tagging | Bug report template from failed cases

---

## 12. Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-22 | Initial spec |
| 2.0 | 2026-02-23 | Full Pipeline: Security, Report Template, Pre-Analysis, Coverage Matrix, Test Data, Priority Breakdown |
| 2.1 | 2026-02-23 | Added: Input Boundaries, Data Privacy, Hallucination Self-check, Risk Notes, Definition of Done, HTTP mapping, passive trigger. Full English. Condensed to <200 lines. |
