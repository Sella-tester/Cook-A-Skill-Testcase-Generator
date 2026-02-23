# SPECIFICATION: SMART TEST CASE GENERATOR SKILL

## 1. General Information
- **Skill Name:** Smart Test Case Generator
- **Author:** Sella
- **AI Persona:** Senior QA Mentor (10+ years experience). The AI acts as a sharp analyst for Seniors while serving as an educational guide for Juniors, maintaining a constructive and professional tone.
- **Goal:** Automatically transform Product Specifications into comprehensive, intelligent, and structured Test Case lists that facilitate high-quality testing and team growth.

## 2. Target Users & Problems to Solve
- **Users:** 
    - Senior QCs: To automate repetitive work and detect complex logic gaps.
    - Junior Testers: To bridge the experience gap through guided test design and clear execution steps.
- **Problem Statement:** 
    - Need for standardized quality across varying experience levels.
    - Reducing time-to-market while increasing test coverage depth.

## 3. Workflow & Validation
3.1. **Input:** User provides a product specification file (.md or .docx).

3.2. **Validation & Pre-process:** 
- AI validates content completeness.
- **Constructive Clarification:** If the spec is ambiguous, AI asks targeted questions explaining the "Business Risk" of the ambiguity to help POs/Devs improve the source.

3.3. **Professional Analysis:** AI applies standard techniques (Equivalence Partitioning, BVA, etc.).

3.4. **Smart Detection:** AI identifies "Logic Gaps" and edge cases with concise "Rational Notes".

3.5. **Generation & Language Sync:** AI automatically matches the Output language to the Input language unless specified otherwise.

3.6. **Review & Refine:** Multi-turn refinement loop for tailored results.

## 4. Input & Output Details
### 4.1. Input Definition (Standard Format)
Markdown structure: Feature Name, User Story, AC, Business Rules, UI/UX descriptions.

### 4.2. Output Structure (Modular & Multi-level Format)
To support both Senior and Junior needs, the output is structured into **Standard Fields** (for export) and **Mentor Fields** (for guidance):

**Standard Fields (Mandatory):**
- **ID, Title, Pre-condition, Steps, Expected Result, Priority.**

**Mentor Fields (Modular/Optional):**
- **Design Logic:** One-liner on the testing technique used.
- **Execution Pro-tips:** Technical guidance (e.g., "Check database for record persistence").
- **Language Choice:** Ability to toggle between English/Vietnamese for the final list.

### 4.3. Acceptance Criteria (AC) for Output
- **Coverage:** 100% AC mapped.
- **Precision:** Steps are concise, unambiguous, and action-oriented.
- **Actionability:** A Junior tester can execute the case without external help.

## 5. Error Handling & Limitations
### 5.1. Error Handling
- **Language Mismatch:** AI alerts if it detects mixed languages that may confuse logic.
- **Over-Complexity:** If a feature is too large, AI suggests breaking it down into sub-modules.

### 5.2. Out of Scope
- Visual/UI Design Verification (pixel-perfect).
- Automated Script Execution.
- Direct logic extraction from non-textual elements in images.

## 6. Business & Team Value
- **Scalability:** Enables teams to scale quality regardless of individual experience levels.
- **Cost Reduction:** Early logic gap detection prevents expensive downstream bug fixing.
- **Knowledge Base:** Encodes Senior expertise into a reusable digital asset.
