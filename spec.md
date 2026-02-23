# SPECIFICATION: SMART TEST CASE GENERATOR SKILL

## 1. General Information
- **Skill Name:** Smart Test Case Generator
- **Author:** Sella
- **AI Persona:** Senior QA Engineer (10+ years experience) acting as a "Mentor". The AI should deliver highly professional technical output while ensuring it is understandable and educational for junior members.
- **Goal:** Automatically transform Product Specifications into comprehensive, intelligent, and structured Test Case lists that serve as both a work tool and a learning reference.

## 2. Target Users & Problems to Solve
- **Users:** 
    - Senior QCs: To accelerate work and ensure zero logic gaps.
    - Junior Testers: To learn professional test design logic and execute tasks with high precision.
- **Problem Statement:** 
    - Manual writing is slow and prone to human error.
    - Knowledge gap between experienced and new team members.
    - Need for a unified, high-standard testing quality across the team.

## 3. Workflow & Validation
3.1. **Input:** User provides a product specification file.
3.2. **Validation (Pre-process):** 
    - AI checks file format and readability.
    - **Smart Clarification:** If logic is ambiguous, AI asks specific questions, explaining *why* the current spec is unclear (e.g., "This flow has no exit condition, which could lead to an infinite loop").
3.3. **Professional Analysis:** AI applies standard techniques (Equivalence Partitioning, Boundary Value Analysis, State Transition).
3.4. **Smart Detection:** AI identifies "Logic Gaps" and edge cases, providing a brief "Rational Note" for each critical case found.
3.5. **Generation:** AI creates structured Test Cases.
3.6. **Accessibility Review:** AI ensures all steps are written in clear, action-oriented language without losing technical depth.
3.7. **Review & Refine:** Iterative feedback loop with the user.
3.8. **Output:** Markdown table or downloadable file.

## 4. Input & Output Details
### 4.1. Input Definition (Standard Format)
Standard Markdown structure: Feature Name, User Story, Acceptance Criteria (AC), Business Rules, UI/UX descriptions.

### 4.2. Output Structure (Enhanced Format)
Each Test Case includes:
- **ID:** (e.g., TC-001).
- **Title:** Action-based description.
- **Pre-condition:** Initial state.
- **Steps:** Clear, numbered execution steps.
- **Expected Result:** Verifiable outcome.
- **Priority:** (P0-P2).
- **Design Logic (Junior Support):** A brief one-liner explaining the testing technique used (e.g., "Boundary Value analysis for 'Age' field").
- **Tester Guidance (Optional):** Pro-tips for execution (e.g., "Monitor network tab for 403 errors").

### 4.3. Acceptance Criteria (AC) for Output
Output must meet: Full Coverage (100% AC), Uniqueness, Clarity, and **Educational Value** (Logical reasoning is evident).

## 5. Error Handling & Limitations
### 5.1. Error Handling
- **Invalid Format:** Alert user.
- **Ambiguous Story:** List questions with "Reasoning" to guide the user in improving the Spec.
- **API Issues:** Standardized retry procedures.

### 5.2. Out of Scope
- Visual/UI Design Verification (pixel-perfect).
- Automated Script Execution (Code-gen for Selenium/Cypress).
- Logic extraction from un-captioned images.

## 6. Business & Team Value
- **Efficiency:** 10-15x faster generation.
- **Risk Mitigation:** Early detection of logic errors.
- **Team Growth:** Acts as an on-job training tool, standardizing Senior-level quality for everyone.
