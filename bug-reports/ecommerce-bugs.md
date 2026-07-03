# Bug Reports — Demo E-Commerce App

**Application:** [automationpractice.com](http://automationpractice.com)
**Tester:** [Your Name]
**Environment:** Chrome 120.0.6099.109 / Windows 11 / 1920×1080

---

## BUG-001 — Cart item count resets to 0 after page refresh

**Severity:** Medium
**Priority:** High
**Status:** Open
**Date Reported:** [Date]
**Related TC:** TC-011

### Description
After adding items to the cart, the cart icon correctly shows the item count. However, after refreshing the page (F5), the cart icon resets to 0 even though the items are still present when navigating to /order.

### Steps to Reproduce
1. Log in with valid credentials
2. Navigate to any product page
3. Add an item to the cart — cart icon shows "1"
4. Press F5 to refresh the current page
5. Observe the cart icon in the top navigation

### Expected Result
Cart icon should continue to display the correct item count (1) after refresh.

### Actual Result
Cart icon resets to 0. Items are still present when navigating to /order directly, but the count is not reflected in the header.

### Evidence
> 📎 *[Attach screenshot here — cart icon before refresh vs. after]*

### Notes
Likely a client-side state management issue. Cart data persists server-side but the header component does not re-initialize cart count on page load. Suggested fix: re-fetch cart count on component mount.

---

## BUG-002 — "Forgot Password" link missing on mobile viewport

**Severity:** Medium
**Priority:** Medium
**Status:** New
**Date Reported:** [Date]
**Related TC:** TC-001

### Description
The "Forgot your password?" link visible on the login page at desktop screen sizes is not rendered on screen widths below 480px.

### Steps to Reproduce
1. Navigate to /login
2. Resize the browser window to 375px width (iPhone simulation) OR open on a real mobile device
3. Observe the login form

### Expected Result
"Forgot your password?" link should be visible below the password field at all screen sizes.

### Actual Result
The link is entirely absent from the DOM at mobile viewport widths. It cannot be scrolled to or found via keyboard navigation.

### Evidence
> 📎 *[Attach screenshot: desktop showing link vs. mobile showing no link]*

### Environment
- Reproduced on: Chrome (DevTools mobile emulation, 375px), Safari iOS 17 (iPhone 15)
- Not reproduced on: Desktop Chrome, Firefox, Edge

### Notes
CSS inspection shows `.forgot-password-link { display: none }` is applied at the 480px media query breakpoint. This appears to be an accidental exclusion rather than intentional design. Accessibility impact: users who forget their password on mobile cannot recover their account.

---

## BUG-003 — Product price displays as $0.00 for out-of-stock items in search results

**Severity:** High
**Priority:** High
**Status:** Open
**Date Reported:** [Date]
**Related TC:** TC-007

### Description
Products that are out of stock display a price of $0.00 in search result cards. The correct price is displayed on the product detail page.

### Steps to Reproduce
1. Search for "Blouse"
2. Filter results by "Out of stock" (if filter available) OR identify an out-of-stock product in results
3. Observe the price displayed in the search result card

### Expected Result
Price should display the actual product price (e.g., $27.00) even when the item is out of stock.

### Actual Result
Price displays as $0.00 in the search result card. Navigating to the product detail page shows the correct price.

### Evidence
> 📎 *[Attach screenshot of search result card showing $0.00 vs. product page showing correct price]*

### Notes
Likely a null-handling issue in the search results rendering logic. When stock_quantity = 0, the price query may be returning null which renders as $0.00. Recommended: validate that price data is fetched independently of stock status.

---

## BUG-004 — Contact form allows submission without selecting a subject heading

**Severity:** Low
**Priority:** Low
**Status:** New
**Date Reported:** [Date]
**Related TC:** TC-019

### Description
The contact form's "Subject Heading" dropdown defaults to a blank selection. The form can be submitted successfully without selecting a subject, which results in a message being sent with no category — making it difficult for support staff to route.

### Steps to Reproduce
1. Navigate to /contact
2. Leave "Subject Heading" at default (blank/unselected)
3. Enter a valid email address
4. Enter a message
5. Click "Send"

### Expected Result
Validation error: "Please select a subject heading."

### Actual Result
Form submits successfully. Confirmation message: "Your message has been successfully sent to our team." Subject is blank in the submitted message.

### Evidence
> 📎 *[Screenshot of successful submission with blank subject]*

### Notes
Low severity — does not break core functionality. However, it creates operational issues for the support team. Simple fix: add `required` attribute to the subject dropdown and server-side validation.

---

## BUG-005 — Page title shows "My Store" on all pages (not page-specific)

**Severity:** Low
**Priority:** Low
**Status:** New
**Date Reported:** [Date]

### Description
The browser tab title shows "My Store" on every page of the application, regardless of which page is loaded. Page-specific titles are never set.

### Steps to Reproduce
1. Navigate to the homepage — title shows "My Store"
2. Navigate to /login — title still shows "My Store"
3. Navigate to a product page — title still shows "My Store"
4. Navigate to /order — title still shows "My Store"

### Expected Result
Page titles should be descriptive and page-specific:
- Homepage: "My Store | Home"
- Login: "Sign In | My Store"
- Product page: "[Product Name] | My Store"
- Cart: "Shopping Cart | My Store"

### Actual Result
All pages display "My Store" as the browser tab title.

### Notes
SEO and accessibility impact. Screen reader users rely on page titles to orient themselves. Also hurts organic search rankings since all pages have identical title tags. Fix: set `<title>` dynamically per route.
