# Product Spec: User Registration

## Feature Name
User Registration

## User Story
As a new user, I want to create an account on the platform so that I can access personalized features and save my preferences.

## Acceptance Criteria

**AC1 — Successful Registration:**
User fills in all required fields (Full Name, Email, Password, Confirm Password) and clicks "Register" → account is created, user receives a confirmation email, and is redirected to the Welcome page.

**AC2 — Password Requirements:**
Password must be 8–64 characters, contain at least 1 uppercase letter, 1 lowercase letter, 1 number, and 1 special character (!@#$%^&*). System shows real-time validation feedback as user types.

**AC3 — Email Validation:**
Email must be a valid format (e.g., user@domain.com). If the email is already registered, system shows: "This email is already in use. Please log in or use a different email."

**AC4 — Confirm Password:**
Confirm Password must match Password. If mismatch, show: "Passwords do not match."

**AC5 — Terms & Conditions:**
User must check the "I agree to the Terms & Conditions" checkbox before registering. If unchecked, the Register button remains disabled.

## Business Rules

**BR1 — Duplicate Prevention:**
System checks for existing accounts by email (case-insensitive). john@example.com and John@Example.com are treated as the same account.

**BR2 — Rate Limiting:**
Maximum 5 registration attempts from the same IP address within 10 minutes. After that, show: "Too many attempts. Please try again later."

**BR3 — Full Name Rules:**
Full Name must be 2–100 characters. Allows letters, spaces, hyphens, and apostrophes. No numbers or special characters.

## UI/UX Notes
- All form fields show inline validation on blur (when user clicks away from field)
- Password strength indicator: Weak (red) / Medium (yellow) / Strong (green)
- "Show Password" toggle icon on Password and Confirm Password fields
- Register button shows loading spinner during API call
- On mobile: fields stack vertically, minimum touch target 44×44px

## Out of Scope
- Social login (Google, Facebook, Apple)
- Two-factor authentication setup during registration
- Profile photo upload during registration
