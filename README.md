# 🧪 Smart Test Case Generator — Cook A Skill Competition

> **AI Skill that transforms product specs into complete, production-quality test case suites — automatically.**

[![Version](https://img.shields.io/badge/Version-2.2-blue)]() [![Author](https://img.shields.io/badge/Author-Sella%20🦞-orange)]() [![Pipeline](https://img.shields.io/badge/Pipeline-6%20Steps-green)]()

---

## 🎯 What It Does

Feed a product spec → get a full test suite in minutes, not hours.

```
[Input: Spec] → [1. Validate] → [2. Analyze & Classify] → [3. Smart Detection]
→ [4. Generate Test Cases] → [5. Report Template + Risk Notes] → [6. Review, Edit & Confirm]
```

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| **7 Test Types** | ✅ Happy Path · ⚠️ Edge Case · ❌ Negative · 🔒 Security · 🖥️ UI/UX · 📱 Mobile · 🔌 API |
| **Smart Detection** | Auto-detects logic gaps, hidden edge cases, and security risks before generation |
| **Pre-Analysis Report** | ACs count, estimated test cases, techniques, and risk summary — before you commit |
| **Hallucination Self-check** | Values not in spec → flagged as `[TBD]` — no fabricated test data |
| **Data Privacy & Masking** | Auto-detects PII (emails, API keys, phone numbers) and offers masking |
| **AC Coverage Matrix** | 100% traceability: every AC mapped to test case IDs |
| **Mentor Fields** | Design Logic + Execution Pro-tips for Junior Testers (toggle off with "Senior mode") |
| **Review Loop** | Approve, edit, reject, or add cases — AI never auto-exports |
| **3 Export Formats** | Markdown (default) · JSON · CSV (TestRail / Jira Xray compatible) |

## 👥 Who It's For

| User | What They Get |
|------|--------------|
| **Senior QC** | Automated generation + edge cases + security risks surfaced instantly |
| **Junior Tester** | Step-by-step guidance with Mentor Fields explaining *why* and *how* |
| **QA Manager** | Standardized output + pre-filled report template — ready for execution |

## ⚡ Before vs After

| | Before (Manual) | After (With Skill) |
|--|----------------|-------------------|
| **Time** | 4–8 hours per feature | 5–10 minutes |
| **Coverage** | Depends on experience | Systematic — up to 7 types every time |
| **Consistency** | Varies by QC | Standardized format |
| **Security** | Often missed | OWASP Top 10 basics built-in |

## 📁 Repository Structure

```
├── spec.md              — Full specification (v2.2) — the blueprint
├── SKILL.md             — AI instructions — how the skill operates
├── SKILL_CARD.md        — One-page summary for the Judging Panel (BGK)
├── QA_Standards.md      — Testing techniques, naming conventions, quality criteria
├── Output_Template.md   — Sample output with Login feature examples
├── demo/
│   ├── sample_input.md  — Sample spec (User Registration feature)
│   └── sample_output.md — Full pipeline output (32 test cases, 7 types)
├── showcase/
│   ├── SYSTEM_PROMPT.md  — Ready-to-paste Custom Instructions for Claude Projects
│   ├── SETUP_GUIDE.md    — Step-by-step Claude Project setup
│   └── DEMO_SCRIPT.md    — Presentation script for the Judging Panel (BGK)
└── README.md             — You are here
```

> 💡 **See it in action!** Check [`demo/`](demo/) for a complete end-to-end example, and [`showcase/`](showcase/) for the Claude Project setup + live demo script.

## 🛠️ Tools / AI Used

- **Claude** (claude.ai) — Core AI for spec analysis and test case generation
- **Markdown** — Primary output format

## 🚫 Out of Scope

- Pixel-perfect UI verification
- Automated test script generation
- Logic extraction from images (Figma, wireframes)
- Performance / load / stress testing
- Deep penetration testing (OWASP basics only)

## 🗺️ Roadmap

- [ ] Xray-compatible export for TestRail / Jira import
- [ ] Auto test data suggestions (valid/invalid sample values)
- [ ] Regression suite tagging
- [ ] Bug report template from failed test cases

---

Developed by **Sella** 🦞
