# Product Spec: E-Commerce Checkout

## Feature Name
Multi-Step Checkout Flow

## User Story
As a logged-in customer, I want to complete a purchase through a multi-step checkout process (Cart Review → Shipping → Payment → Order Confirmation) so that I can buy products and have them delivered to my address.

## Acceptance Criteria

**AC1 — Cart Review Step:**
User sees a summary of all items in cart: product name, thumbnail image, quantity, unit price, subtotal per item, and cart total. User can update quantity (1–99) or remove items. Cart total updates in real-time. If cart is empty, show: "Your cart is empty. Start shopping!" with a link to the homepage.

**AC2 — Shipping Address Step:**
User enters or selects a saved shipping address. Required fields: Full Name, Phone Number, Address Line 1, City, Province/State, Postal Code, Country. Optional: Address Line 2, Delivery Notes. After entering address, system calculates shipping fee and displays estimated delivery date (3–7 business days for Standard, 1–2 business days for Express).

**AC3 — Shipping Method Selection:**
User selects one shipping method: Standard (free for orders ≥ 500,000 VND, otherwise 30,000 VND) or Express (60,000 VND flat rate). Default: Standard. Shipping fee updates the order total immediately.

**AC4 — Payment Step:**
User selects one payment method: Credit/Debit Card (Visa, Mastercard, JCB), Bank Transfer, or Cash on Delivery (COD). For Credit/Debit Card: user enters Card Number, Cardholder Name, Expiry Date (MM/YY), and CVV. System validates card number format using Luhn algorithm. For Bank Transfer: system generates a unique transaction reference code and displays bank account details. For COD: no additional input required — maximum order value for COD is 5,000,000 VND.

**AC5 — Discount Code:**
User can enter one discount code per order. System validates the code and shows: discount type (percentage or fixed amount), discount value, and new total. Invalid code shows: "Invalid discount code. Please check and try again." Expired code shows: "This discount code has expired." Already-used code shows: "This discount code has already been used." Discount cannot reduce total below 0 VND — minimum payable amount is 1,000 VND.

**AC6 — Order Summary & Confirmation:**
Before final submission, user sees: all items, shipping address, shipping method + fee, payment method, discount applied (if any), and final total (items + shipping - discount). User clicks "Place Order" → system processes payment → on success: redirect to Order Confirmation page showing order number, estimated delivery date, and a "Track Order" link. On failure: show error "Payment failed. Please try again or choose a different payment method."

**AC7 — Inventory Check:**
Before processing payment, system verifies stock availability for all items. If any item is out of stock, show: "Sorry, [Product Name] is no longer available. Please remove it from your cart to continue." Order cannot proceed until all items are in stock.

**AC8 — Order Confirmation Email:**
After successful order placement, system sends a confirmation email containing: order number, item list with prices, shipping address, estimated delivery date, payment method used, and total paid. Email sent within 2 minutes of order completion.

## Business Rules

**BR1 — Minimum Order Value:**
Minimum order value is 50,000 VND (before shipping and discounts). Orders below this show: "Minimum order value is 50,000 VND. Please add more items."

**BR2 — Discount Stacking:**
Only one discount code per order. System rejects the second code with: "Only one discount code can be applied per order."

**BR3 — Price Lock:**
Item prices are locked at the time of adding to cart. Price changes on the product page do not affect items already in cart during the same session.

**BR4 — Session Timeout:**
Checkout session expires after 30 minutes of inactivity. User is redirected to Cart Review with message: "Your session has expired. Please review your cart and try again." Cart items are preserved.

**BR5 — Concurrent Purchase:**
If two users attempt to buy the last unit of the same product simultaneously, the first to complete payment gets the item. The second user receives: "Sorry, [Product Name] just sold out."

**BR6 — Card Payment Retry:**
Maximum 3 payment attempts per order. After 3 failures, payment method is locked for that order with message: "Too many failed attempts. Please try a different payment method or contact support."

**BR7 — Order Number Format:**
Order number format: `ORD-YYYYMMDD-XXXXX` where XXXXX is a sequential 5-digit number per day (e.g., ORD-20260224-00001).

## UI/UX Notes
- Multi-step progress bar at top: Cart → Shipping → Payment → Confirmation. Current step highlighted. Completed steps show green checkmark.
- "Back" button on each step returns to previous step without losing data.
- Order summary sidebar visible on all steps (desktop). On mobile: collapsible accordion.
- All monetary values formatted as Vietnamese Dong: "₫500,000" with thousand separators.
- Card Number field auto-formats with spaces: "4111 1111 1111 1111"
- CVV field: masked input (shows dots)
- Loading overlay with spinner during payment processing ("Processing your payment...")
- Toast notification for discount code success/failure
- Disable "Place Order" button after first click to prevent double submission

## API Endpoints
- `POST /api/orders` — Create order
- `POST /api/payments/process` — Process payment
- `GET /api/shipping/calculate` — Calculate shipping fee
- `POST /api/discounts/validate` — Validate discount code
- `GET /api/inventory/check` — Check stock availability

## Out of Scope
- Guest checkout (login required)
- Multi-currency support
- Installment payment plans
- Gift wrapping or gift messages
- Saved payment methods (card on file)
- Address auto-complete (Google Places API)
