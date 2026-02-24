# Claude Project — Setup Guide

> Step-by-step guide to set up the Smart Test Case Generator in Claude Pro (Projects).

---

## Prerequisites

- Claude Pro account (claude.ai)
- Files from this repository downloaded

---

## Setup Steps

### Step 1 — Create a New Project

1. Go to [claude.ai](https://claude.ai)
2. Click **"Projects"** in the left sidebar
3. Click **"Create Project"**
4. Name it: **Smart Test Case Generator v2.2**
5. Description (optional): "AI Skill: Transform product specs into complete test case suites"

### Step 2 — Set Custom Instructions

1. In the Project, click **"Set custom instructions"** (or the ⚙️ icon)
2. Open `showcase/SYSTEM_PROMPT.md` from this repository
3. **Copy the entire content** and paste it into the Custom Instructions field
4. Click **Save**

### Step 3 — Upload Project Knowledge

Upload the following files as **Project Knowledge** (drag & drop or click "Add content"):

| File | Purpose | Required? |
|------|---------|-----------|
| `QA_Standards.md` | Testing techniques, naming conventions, priority rules | ✅ Yes |
| `Output_Template.md` | Output format reference with examples | ✅ Yes |
| `spec.md` | Full specification (for AI reference) | 🟡 Optional |

> **Why these files?** The System Prompt tells AI *what to do*. Knowledge files give it *reference material* for consistent formatting and technique application.

### Step 4 — Verify Setup

Start a new conversation in the Project and type:

```
Hi, what can you do?
```

**Expected response:** The AI should describe itself as a Senior QA Mentor, explain the 6-step pipeline, and ask for a product spec.

**If it doesn't** → check that Custom Instructions were saved correctly.

### Step 5 — Test Run

Paste the sample spec from `demo/sample_input.md`:

```
# Product Spec: User Registration
...
```

**Expected behavior:**
1. AI validates the input ✅
2. Shows Pre-Analysis Report with estimated test cases
3. Asks "Shall I proceed?"
4. You say "Yes"
5. Generates full test case suite (30+ test cases)
6. Shows Report Template + Risk Notes
7. Shows AC Coverage Matrix
8. Asks for review (approve/edit/reject)

---

## Project Settings Checklist

| Setting | Recommended |
|---------|-------------|
| Model | Claude Sonnet 4 (or latest) |
| Custom Instructions | `SYSTEM_PROMPT.md` content pasted |
| Knowledge Files | `QA_Standards.md` + `Output_Template.md` |
| Artifacts | Enabled ✅ (for table rendering) |

---

## Troubleshooting

| Issue | Solution |
|-------|---------|
| AI doesn't follow the pipeline | Re-check Custom Instructions — ensure full SYSTEM_PROMPT.md is pasted |
| Output format inconsistent | Upload `Output_Template.md` as Knowledge — AI references it for formatting |
| AI hallucinates values | This is expected to be caught by Hallucination Self-check — verify `[TBD]` flags appear |
| Test cases too generic | Add more detail to your input spec — more specific ACs = more specific test cases |
| AI generates in wrong language | Say "Output in English" or "Output in Vietnamese" to override |

---

## Tips for Best Results

1. **Spec quality matters.** More detailed ACs → better test cases. Use the recommended structure: Feature name → User Story → ACs → Business Rules → UI/UX Notes → Out of Scope.
2. **One feature per conversation.** For multi-feature specs, start a new chat per feature.
3. **Use the review loop.** Don't just accept — edit, add, or reject cases. The AI learns from your feedback within the conversation.
4. **Try "Senior mode"** if you don't need Mentor Fields — cleaner output for experienced QCs.
5. **Export format matters.** Use CSV for TestRail/Jira import, JSON for automation, Markdown for review.
