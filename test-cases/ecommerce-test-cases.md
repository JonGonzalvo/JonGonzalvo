# Test Cases — Demo E-Commerce App

**Application:** [automationpractice.com](http://automationpractice.com)
**Tester:** [Your Name]
**Date:** [Month Year]
**Environment:** Chrome [version] / Windows 11

---

## Module: User Authentication

| TC ID | Title | Preconditions | Steps | Expected Result | Status |
|---|---|---|---|---|---|
| TC-001 | Valid login with registered credentials | User has a registered account; is on /login | 1. Enter registered email 2. Enter correct password 3. Click Sign In | User is redirected to My Account page; name visible in header | Pass |
| TC-002 | Login with incorrect password | User is on /login | 1. Enter valid email 2. Enter wrong password 3. Click Sign In | Error: "Authentication failed." No redirect. | Pass |
| TC-003 | Login with unregistered email | User is on /login | 1. Enter email not in system 2. Enter any password 3. Click Sign In | Error: "Authentication failed." No redirect. | Pass |
| TC-004 | Login with empty fields | User is on /login | 1. Leave email and password blank 2. Click Sign In | Validation error on both fields; no submit attempt. | Pass |
| TC-005 | Login with invalid email format | User is on /login | 1. Enter "notanemail" in email field 2. Enter any password 3. Click Sign In | Validation error: "Invalid email address." | Pass |
| TC-006 | Successful logout | User is logged in | 1. Click account name in header 2. Click "Sign Out" | User is redirected to homepage; account name no longer visible in header. | Pass |

---

## Module: Product Search

| TC ID | Title | Preconditions | Steps | Expected Result | Status |
|---|---|---|---|---|---|
| TC-007 | Search for existing product by name | User is on homepage | 1. Type "Dress" in search bar 2. Press Enter | Results page loads; products matching "Dress" are displayed. | Pass |
| TC-008 | Search returns no results for nonsense query | User is on homepage | 1. Type "xyzxyz123" in search bar 2. Press Enter | "No results were found" message displayed; no product cards shown. | Pass |
| TC-009 | Search is not case-sensitive | User is on homepage | 1. Search "dress" (lowercase) 2. Note results 3. Search "DRESS" (uppercase) | Both searches return identical results. | Pass |
| TC-010 | Search with special characters | User is on homepage | 1. Enter "< > & !" in search bar 2. Press Enter | App handles gracefully; no error or crash; shows 0 results or relevant results. | Pass |

---

## Module: Add to Cart

| TC ID | Title | Preconditions | Steps | Expected Result | Status |
|---|---|---|---|---|---|
| TC-011 | Add single item to cart | User is logged in; on a product page | 1. Select size and color if required 2. Click "Add to cart" | Modal confirms item added; cart icon count increases to 1. | Pass |
| TC-012 | Add multiple different items to cart | User is logged in | 1. Add Product A to cart 2. Add Product B to cart 3. Navigate to cart | Both products appear in cart with correct names, quantities, and prices. | Pass |
| TC-013 | Update quantity in cart | User has 1 item in cart | 1. Navigate to /order 2. Change quantity to 3 3. Click update/recalculate | Quantity updates; subtotal recalculates correctly (unit price × 3). | Pass |
| TC-014 | Remove item from cart | User has items in cart | 1. Navigate to /order 2. Click delete icon next to an item | Item is removed; cart total updates; if last item, cart shows empty. | Pass |
| TC-015 | Cart persists after logout and re-login | User has items in cart | 1. Add item to cart 2. Log out 3. Log back in 4. Check cart | Cart still contains the previously added item. | Blocked |

---

## Module: Checkout

| TC ID | Title | Preconditions | Steps | Expected Result | Status |
|---|---|---|---|---|---|
| TC-016 | Complete checkout as logged-in user | User is logged in; has item in cart; has a saved address | 1. Navigate to cart 2. Proceed to checkout 3. Confirm address 4. Select shipping 5. Agree to terms 6. Confirm order | Order confirmation page displayed with order number. | Pass |
| TC-017 | Checkout blocked without agreeing to terms | User is logged in; at payment step | 1. Reach the payment/terms step 2. Do NOT check terms checkbox 3. Click "Order with an obligation to pay" | Error: "You must agree to the Terms of Service." Order not placed. | Pass |
| TC-018 | Guest checkout (if supported) | User is NOT logged in; has item in cart | 1. Proceed to checkout as guest 2. Enter valid email and shipping address 3. Complete order | Order confirmation displayed; confirmation email sent to provided address. | N/A |

---

## Module: Contact Form

| TC ID | Title | Preconditions | Steps | Expected Result | Status |
|---|---|---|---|---|---|
| TC-019 | Submit contact form with valid data | User is on /contact | 1. Select subject heading 2. Enter registered email 3. Type message (min. 1 char) 4. Click Send | Success: "Your message has been successfully sent to our team." | Pass |
| TC-020 | Submit contact form with empty message | User is on /contact | 1. Select subject 2. Enter email 3. Leave message blank 4. Click Send | Validation error: "The message cannot be blank." | Pass |
| TC-021 | Submit contact form with invalid email | User is on /contact | 1. Select subject 2. Enter "bademail" 3. Enter message 4. Click Send | Validation error on email field. | Pass |

---

## Notes

- TC-015 marked **Blocked** — session persistence behavior varies; log a bug if cart clears on re-login unexpectedly.
- TC-018 marked **N/A** — the demo app does not support guest checkout.
- All tests executed in Chrome on Windows 11. Cross-browser testing (Firefox, Safari, Edge) is a separate pass.
