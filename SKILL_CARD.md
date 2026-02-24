# Skill Card — Smart Test Case Generator

> One-page summary for presentation to the Judging Panel (BGK).

---

| Field | Detail |
|-------|--------|
| **Skill Name** | Smart Test Case Generator |
| **Author** | Sella |
| **Version** | 2.2 |
| **Approach** | Full Pipeline (6 Steps) |

---

## What This Skill Automates

Reads a product spec → Validates & analyzes Acceptance Criteria → Detects logic gaps & security risks → Generates a complete test case suite (up to 7 types) → Exports a pre-filled Report Template + Risk Notes → Interactive review loop before final export.

**6-Step Pipeline:**
```
[Input: Spec] → [1. Validate] → [2. Analyze & Classify] → [3. Smart Detection]
→ [4. Generate Test Cases] → [5. Report Template + Risk Notes] → [6. Review, Edit & Confirm]
```

---

## Before vs After

| | Before (Manual) | After (With Skill) |
|--|----------------|-------------------|
| **Process** | QC reads spec manually → thinks through each test case individually | Feed spec to AI → receive Pre-Analysis Report + full test case suite + Report Template |
| **Time** | 4–8 hours for one complex feature | 5–10 minutes — QC only reviews and supplements |
| **Coverage** | Depends on individual experience — edge cases and security cases are often missed | Systematic coverage: up to 7 test types (Happy Path, Edge, Negative, Security, UI/UX, Mobile, API) |
| **Consistency** | Each QC writes in a different format and style | Standardized output format regardless of who runs the skill |
| **Junior support** | Junior Testers often need guidance from Seniors | Mentor Fields (Design Logic + Execution Pro-tip) embedded in every test case |
| **Data safety** | PII in specs may go unnoticed | Auto-detect & mask sensitive data before processing |
| **Reliability** | AI may hallucinate test data | Hallucination Self-check — unverified values flagged as `[TBD]` |

---

## Test Type Coverage

| Type | When Generated |
|------|---------------|
| ✅ Happy Path | Always |
| ⚠️ Edge Case | Always |
| ❌ Negative | Always |
| 🔒 Security | When feature has auth / forms / user data |
| 🖥️ UI/UX | When spec describes UI behavior |
| 📱 Mobile | When platform includes mobile |
| 🔌 API | When feature has API endpoints |

---

## Key Features

- **Pre-Analysis Report** — ACs, Logic Gaps, Security Risks, and estimated test case counts before generation
- **AC Coverage Matrix** — 100% traceability from ACs → Test Cases
- **Data Privacy & Masking** — Auto-detects PII (emails, API keys, phone numbers) and offers masking
- **Hallucination Self-check** — Values not in spec → `[TBD]` with review flag
- **Review Loop** — User approves, edits, or rejects cases before export (AI never auto-exports)
- **Mentor Fields** — Design Logic + Pro-tips for Junior Testers (toggle off with "Senior mode")

---

## Export Formats

| Format | Use Case |
|--------|----------|
| **Markdown** *(default)* | Human review, GitHub, Notion |
| **JSON** | API integration, test management tools |
| **CSV** | Excel, TestRail / Jira Xray import |

---

## Tools / AI Used

- **Claude** (claude.ai) — Core AI for spec analysis and test case generation
- **Markdown** — Primary output format for test cases and report template

---

## Skill Limitations

- Cannot read visual specs (Figma screenshots, image-only files)
- Security coverage is OWASP Top 10 basics only — not a replacement for dedicated penetration testing
- Cannot auto-sync with test management tools (TestRail, Jira Xray)
- Output is for manual execution — no automated script generation

---

## Roadmap (Next Steps)

- Xray-compatible export for TestRail / Jira import
- Automatic test data suggestions per test case (valid/invalid sample values)
- Regression suite tagging — flag which cases are suitable for regression runs
- Auto-generate bug report template from failed test cases
