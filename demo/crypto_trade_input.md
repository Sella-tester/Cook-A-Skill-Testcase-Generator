# Product Spec: Crypto Exchange Order Book (Buy/Sell)

## Feature Name
Spot Trading Order Book (Limit & Market Orders)

## User Story
As a trader, I want to place Buy and Sell orders (Limit or Market) on the spot market so that I can trade cryptocurrencies at my preferred price or instantly at market price.

## Acceptance Criteria

**AC1 — Limit Order Placement:**
User selects "Limit", enters "Price" and "Amount". System calculates "Total" (Price × Amount). System validates that User has enough "Available Balance" (including trading fees). Clicking "Buy" or "Sell" places the order into the Order Book. Order status: `Open`.

**AC2 — Market Order Placement:**
User selects "Market", enters "Amount" (for Sell) or "Total" (for Buy). System executes the order instantly against the best available price(s) in the Order Book. Order status: `Filled`.

**AC3 — Balance Validation & Locking:**
When a Limit Order is placed, the required balance is moved from "Available" to "Frozen/In-Order". User cannot spend frozen funds. If balance < (Total + Fee), show error: "Insufficient balance".

**AC4 — Precision & Step Size:**
System must enforce asset-specific precision. Example (BTC/USDT): Price precision = 2 decimals (0.01), Amount precision = 5 decimals (0.00001). Minimum order total = 5 USDT. Invalid precision/total shows: "Invalid amount/price step."

**AC5 — Order Matching (Trade Execution):**
Buy Limit Order matches when `Market Ask Price <= Order Price`. Sell Limit Order matches when `Market Bid Price >= Order Price`. Partial fills are allowed. Order status updates: `Open` -> `Partially Filled` -> `Filled`.

**AC6 — Order Cancellation:**
User can cancel any `Open` or `Partially Filled` order. Upon cancellation: remaining frozen funds are returned to "Available balance", order status: `Canceled`.

**AC7 — Trading Fees:**
Standard fee is 0.1% per trade. If user holds platform token (e.g., CF token), they can opt to pay fees with it for a 25% discount (0.075%). Fee is deducted from the destination asset (for Buy: deduct from asset bought; for Sell: deduct from currency received).

## Business Rules

**BR1 — Price Protection (Fat Finger):**
Limit orders > 10% away from current market price require a second confirmation: "Your price is 10% higher/lower than market price. Confirm order?"

**BR2 — Post-Only Order:**
Optional setting for Limit orders. If enabled, the order is only placed if it enters the order book as a Maker (does not match instantly). If it would match instantly, the system cancels it automatically.

**BR3 — Minimum Order Value:**
The total value (Price × Amount) must be ≥ 5 USDT.

**BR4 — Maximum Order Value:**
Individual orders cannot exceed 1,000,000 USDT for security reasons.

**BR5 — Self-Trade Prevention:**
A user cannot match against their own open orders. If a match would occur, the incoming order is canceled or partially filled up to the self-trade point.

**BR6 — Maintenance Mode:**
During system maintenance, order placement and cancellation are disabled. System returns: "Trading is temporarily suspended for maintenance." Existing Open orders are preserved but cannot be matched or canceled until maintenance ends.

**BR7 — Fee Rounding & Calculation:**
Trading fees are calculated with 8 decimal places and rounded UP (ceiling) to the smallest unit of the asset. If the User's platform token (CF) balance is insufficient to cover the full discounted fee, the system defaults to the standard 0.1% fee deducted from the primary asset.

**BR9 — Matching Priority:**
Orders at the same price level are matched based on **Time Priority (FIFO)**. The order that was placed earlier in the system will be executed first.

**BR10 — Trading Dust:**
Any remaining asset balance smaller than the minimum "Step Size" (Precision) defined in AC4 is considered "Dust". Dust cannot be used for trading but can be converted to platform tokens (CF) via the "Convert Small Balance" feature.

**BR11 — Fee Discount Check Time:**
The system checks for sufficient platform token (CF) balance at the exact moment of **Trade Execution**, not at the time of Order Placement. If balance is insufficient during execution, standard 0.1% fees apply.

**BR12 — Partial Fill Locking Logic:**
For partially filled orders, the portion of the balance corresponding to the remaining unfulfilled amount remains "Frozen". Only the portion corresponding to the executed trade (plus fees) is permanently deducted.

**BR13 — Market Order Slippage Protection:**
To protect against extreme volatility, Market Orders are subject to a maximum 5% slippage limit relative to the last traded price. If execution would result in more than 5% slippage, the order is only filled up to the 5% mark and the rest is canceled.

## UI/UX Notes
- Real-time Order Book display (Red for Asks, Green for Bids).
- Percentage buttons (25%, 50%, 75%, 100%) to auto-calculate Amount based on Available Balance.
- Slider for quick Amount/Total adjustment.
- "Open Orders" and "Trade History" tables below the trading interface.
- Price/Amount fields show red border on validation failure.
- Success toast: "Order placed successfully."

## API Endpoints
- `POST /api/v1/order` — Place new order
- `DELETE /api/v1/order/{orderId}` — Cancel order
- `GET /api/v1/orderbook/{symbol}` — Fetch order book
- `GET /api/v1/balance` — Fetch user balance

## Out of Scope
- Futures/Margin trading (Leverage).
- Stop-Limit / OCO (One-Cancels-the-Other) orders.
- API Key trading (Third-party bot access).
- Advanced charting (TradingView integration).
