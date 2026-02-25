# Product Spec: Anti-Click-Fraud Protection System

## Feature Name
Real-Time Ad Fraud Detection & IP Blocking (Google & Meta Ads)

## User Story
As a Digital Marketer, I want to automatically detect and block fraudulent clicks (bots, competitors, click farms) on my Google and Facebook ads so that I can save my advertising budget and increase lead quality.

## Acceptance Criteria

**AC1 — Real-Time Tracking & Fingerprinting:**
The system must track every click via a lightweight JS snippet. It must capture: IP Address, Device Fingerprint (OS, Browser, Hardware ID), Referrer, and GCLID/FBCLID.

**AC2 — Fraud Detection Rules:**
System flags a click as "Fraudulent" if it triggers any of these conditions:
- **Frequency:** More than 3 clicks from the same IP/Device within 60 seconds.
- **Blacklist:** IP is found in the Global Fraud Database.
- **Bot Behavior:** Non-human mouse movements or zero-second page stays.
- **VPN/Proxy:** Click originates from a known hosting provider or VPN exit node.

**AC3 — Automatic Blocking (API Integration):**
Once flagged, the system must automatically add the IP address to the "Excluded IP" list in the linked Google Ads or Meta Ads account via API within 30 seconds.

**AC4 — Dashboard & Reporting:**
User sees a real-time dashboard showing: Total Clicks, Blocked Clicks, Money Saved (Estimated), and Fraud Map (Geographic location of attackers).

**AC5 — Threshold Management:**
User can customize blocking sensitivity (e.g., "Standard", "Aggressive", or "Custom rules").

## Business Rules
- **BR1 — Exclusion Limit:** Google Ads allows max 500 IP exclusions per campaign. System must rotate the list (FIFO) if the limit is reached.
- **BR2 — Whitelist:** Clicks from internal office IPs or verified partner IPs must never be blocked.
- **BR3 — Data Privacy:** System must comply with GDPR/CCPA. PII (Personal Identifiable Information) must be masked in logs.
- **BR4 — API Failover:** If Google/Meta API is down, system must queue blocking requests and retry every 5 minutes.

## UI/UX Notes
- Real-time "Flash" notification when a bot is blocked.
- Clear status indicator: "Connected" or "API Error" for each Ad platform.
- Mobile-responsive dashboard for on-the-go monitoring.

## API Endpoints
- `POST /api/v1/log-click` — Ingest click data from JS snippet.
- `POST /api/v1/ad-platforms/block-ip` — Outbound call to Google/Meta API.
- `GET /api/v1/stats/summary` — Fetch dashboard data.
