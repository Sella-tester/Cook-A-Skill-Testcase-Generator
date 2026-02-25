# Spec Writing Guide — Smart Test Case Generator

> **Goal:** Help AI deeply understand your business logic, detect the right logic gaps, and generate a test suite with 100% coverage.

---

## 1. The Ideal Spec Structure

To ensure the AI doesn't have to "guess," present your specification in the following order:

1.  **Feature Name:** A clear, concise title.
2.  **User Story:** Role + Action + Value (*As a... I want to... So that...*).
3.  **Acceptance Criteria (AC):** The conditions that must be met (The most critical part).
4.  **Business Rules (BR):** Logic rules, calculation formulas, constraints, and limits.
5.  **UI/UX Notes:** Description of the interface, error messages, and loading states.
6.  **API Endpoints (Optional):** Enables the AI to generate system integration and technical cases.
7.  **Out of Scope:** What the feature *will not* do (prevents unnecessary test cases).

---

## 2. Writing "Sharp" Acceptance Criteria (AC)

The AI performs best when ACs are **testable** and observable.

*   **❌ Avoid:** "The system should verify that the password is secure."
*   **✅ Use:** "AC: When the user clicks Register, the system validates the password is at least 8 characters, containing 1 uppercase and 1 special character. If invalid, display a red error message below the field."

---

## 3. Include Business Rules (The "Hidden" Logic)

This is where the AI finds **Edge Cases**. Be explicit with numbers and logic:
*   **Limits:** "Maximum 5 attempts per 10 minutes," "Minimum order value is 50,000 VND."
*   **Rounding:** "Shipping fees are rounded up to the nearest thousand."
*   **Priority:** "Order matching priority is based on Time (FIFO)."

---

## 4. Triggering Mobile & API Cases

If you want the AI to generate specific suites for mobile or technical layers, include these keywords:
*   **Mobile:** Mention "Touch targets (min 44x44px)," "Mobile app," "FaceID/Fingerprint," or "Interrupts (calls/notifications)."
*   **API:** List endpoints like `POST /api/v1/transfer` and mention "Status codes," "Idempotency," or "JSON schema."
*   **UI/UX:** Mention "Loading spinner," "Toast notifications," or "Button disabled states."

---

## 5. Pro-Tips for Best Results

*   **Use Markdown:** Use `#`, `##`, `-`, and `|` (tables) to help the AI parse information blocks effectively.
*   **Use Technical Terms:** Don't hesitate to use terms like "Luhn algorithm," "Race condition," "JWT," or "CSRF." The AI is trained on professional technical contexts.
*   **Describe States:** For example, an order flow: `New` -> `Paid` -> `Shipped` -> `Completed`. The AI will automatically generate a State Transition matrix.

---

### 💡 The "Self-Test" for your Spec:
Before running the Skill, ask yourself: *"If I gave this document to a new tester, could they execute the tests without asking me a single question?"* If the answer is **Yes**, the AI output will be perfect.
