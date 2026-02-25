# Product Spec: Mobile Banking Fund Transfer

## Feature Name
Internal & Interbank Fund Transfer (24/7)

## User Story
As a verified bank customer, I want to transfer money to another account (same bank or different bank) via the mobile app so that I can send funds instantly and securely anytime.

## Acceptance Criteria

**AC1 — Internal Transfer (Same Bank):**
User enters Account Number. System auto-fetches and displays Recipient Name for verification. User enters Amount and optional Remark. System validates balance and places transfer.

**AC2 — Interbank Transfer (NAPAS 24/7):**
User selects Bank from list and enters Account Number. System auto-fetches Recipient Name via NAPAS gateway. Fees are displayed before confirmation.

**AC3 — Security Authentication:**
All transfers > 1,000,000 VND require Biometric (FaceID/Fingerprint) or Smart OTP. Transfers > 500,000,000 VND per day require Digital Certificate (Soft Token).

**AC4 — Transaction Limits:**
Minimum transfer: 10,000 VND. Maximum per transaction: 500,000,000 VND. Maximum per day: 2,000,000,000 VND.

**AC5 — Transaction Status:**
On success: Show "Success" screen with transaction ID, time, and option to share receipt. On failure (e.g., system busy): Show "Failed" screen with specific reason.

## Business Rules
- **BR1 — Insufficient Balance:** Transfer amount + Fee must be <= Available Balance.
- **BR2 — Fee Logic:** Internal is free. Interbank is 2,200 VND (fixed).
- **BR3 — Idempotency:** If the user clicks "Confirm" twice within 5 seconds, the system must only process the transaction once.
- **BR4 — Maintenance:** NAPAS transfers may be disabled if the central gateway is down.

## UI/UX & Mobile Specifics
- Progress bar: Input -> Confirm -> OTP -> Result.
- FaceID/TouchID prompt appears automatically for large amounts.
- App must handle incoming calls or app-switching during OTP entry.
- Offline mode: If internet is lost during "Confirm", app should retry 3 times then show "Connection Error".

## API Endpoints
- `GET /api/v1/recipient/{bankId}/{accountNumber}` — Fetch recipient name
- `POST /api/v1/transfer/execute` — Process the fund transfer (Requires Idempotency-Key)
