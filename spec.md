# SPECIFICATION: SMART TEST CASE GENERATOR SKILL

## 1. General Information
- **Skill Name:** Smart Test Case Generator
- **Author:** Sella
- **AI Persona:** Senior QA Engineer (10+ years experience) specializing in logical analysis and comprehensive test coverage.
- **Goal:** Automatically transform Product Specifications (Product Spec) into comprehensive, intelligent, and structured Test Case lists.

## 2. Target Users & Problems to Solve
- **Users:** QC Engineers / Testers / QA Managers.
- **Problem Statement:** 
    - Manual test case writing is time-consuming (averaging several hours to a full day for a large module).
    - High risk of missing edge cases or ambiguous logic hidden within the Spec.
    - Inconsistent test case formats among different team members.

## 3. Workflow & Validation
3.1. **Input:** User provides a product specification file.
3.2. **Validation (Pre-process):** 
    - AI checks file format and content readability.
    - If the story is unclear or lacks logic, AI will pause and ask for clarification (Error Handling).
3.3. **Analysis:** AI parses logical flows, functional requirements, and data constraints based on standard testing techniques (Equivalence Partitioning, Boundary Value Analysis).
3.4. **Smart Detection:** AI proactively identifies ambiguities/logic gaps in the Spec and lists potential edge cases.
3.5. **Generation:** AI generates the test case list based on a standardized structure.
3.6. **Review & Refine:** User reviews output; AI provides a "Refinement Mode" to update cases based on feedback.
3.7. **Output:** Returns Test Cases in Markdown table or downloadable file.

## 4. Input & Output Details
### 4.1. Input Definition (Standard Format)
To ensure high-quality output, the input should follow this Markdown structure:
- **Feature Name:** Clear title.
- **Context/User Story:** "As a [user], I want to [action] so that [value]."
- **Acceptance Criteria (AC):** Numbered list of conditions.
- **Technical Constraints:** Rules, data types, limits.
- **UI/UX:** Description of elements (if any).

### 4.2. Output Structure (Test Case Format)
Each Test Case includes:
- **ID:** (e.g., TC-001).
- **Title:** Action-based description.
- **Pre-condition:** State before testing.
- **Steps:** Numbered execution steps.
- **Expected Result:** Clear, verifiable outcome.
- **Priority:** (P0: Critical, P1: Major, P2: Minor).
- **Type:** (Happy Path / Edge Case / Negative Case).

### 4.3. Acceptance Criteria (AC) for Output
The generated output is considered "Standard" if it meets:
- **Full Coverage:** 100% of User Story ACs are covered.
- **Uniqueness:** No duplicate test cases.
- **Clarity:** Steps are written in simple, imperative language.
- **Independence:** Each test case can be executed independently.

## 5. Error Handling & Limitations
### 5.1. Error Handling
- **Invalid Format:** Alert user if file is not .md or .docx.
- **Ambiguous Story:** If AI "confidence" is < 80% regarding logic, it must list specific questions for the user instead of guessing.
- **API/Connection Issues:** Standardized retry message with local state backup.

### 5.2. Out of Scope (What this tool DOES NOT do)
- **UI/Visual Testing:** Cannot verify pixel-perfect design or colors.
- **Automated Execution:** This tool only *generates* cases, it does not *run* them.
- **Image Processing:** Cannot read logic directly from screenshots/mockups (text description required).

## 6. Business Value
- **Efficiency:** Reduces writing time by 10-15x.
- **Risk Mitigation:** Early detection of Spec issues reduces dev-rework costs.
- **Consistency:** Uniform quality across the whole testing team.
