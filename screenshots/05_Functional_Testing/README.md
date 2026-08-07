# Functional Testing

## Objective

Verify the core functionality of the OWASP Juice Shop application to ensure that business workflows operate as expected under normal user interaction. The assessment focused on validating shopping, checkout, address management, payment card management, feedback submission, and complaint submission functionality while documenting observed application behavior.

---

## Functional Areas Tested

- User Registration
- User Login
- Product Search
- Add to Basket
- Basket Management
- Address Management
- Payment Card Management
- Checkout Process
- Order History
- Order Tracking
- Feedback Submission
- Complaint Submission

---

# Testing Methodology

## 1. Basket Management

The shopping basket functionality was tested by performing normal user operations.

Performed tests:

- Add product to basket
- Increase product quantity
- Decrease product quantity
- Remove product
- Empty basket

### Observation

All basket operations functioned as expected.

The application successfully updated basket contents and reflected the changes immediately within the user interface.

---

## 2. Address Management

The address management functionality was verified using multiple address records.

Performed tests:

- Create new address
- Delete address
- Create duplicate address

### Observation

Address creation and deletion functioned successfully.

The application allowed multiple duplicate address entries without validation.

---

## 3. Payment Card Management

Payment card functionality was tested using valid payment card information.

Performed tests:

- Add payment card
- Delete payment card
- Create duplicate payment cards

### Observation

Card creation and deletion operated successfully.

The application allowed duplicate payment card entries.

No card editing functionality was available in the tested application.

---

## 4. Checkout Process

The checkout workflow was verified from product selection through order completion.

Performed tests:

- Checkout with valid basket
- Verify order creation
- Verify order history
- Verify order tracking

### Observation

The checkout process completed successfully.

Orders were generated correctly and appeared within the user's order history.

Order tracking functionality successfully displayed order details.

---

## 5. Feedback Submission

Feedback functionality was tested using valid user input.

Performed tests:

- Submit customer feedback

### Observation

Feedback submissions were accepted successfully.

Submitted feedback was not visible through the application interface during testing, preventing verification of persistent storage through the UI.

---

## 6. Complaint Submission

Complaint functionality was tested using valid complaint information.

Performed tests:

- Submit complaint

### Observation

Complaint submissions were accepted successfully.

Submitted complaints were not visible through the application interface during testing, preventing verification of persistent storage through the UI.

---

# Summary of Tests Performed

| Test | Result |
|-------|--------|
| User Registration | ✅ Successful |
| User Login | ✅ Successful |
| Product Search | ✅ Successful |
| Add Product to Basket | ✅ Successful |
| Update Basket Quantity | ✅ Successful |
| Remove Product from Basket | ✅ Successful |
| Empty Basket | ✅ Successful |
| Create Address | ✅ Successful |
| Delete Address | ✅ Successful |
| Duplicate Address Creation | ⚠️ Allowed |
| Create Payment Card | ✅ Successful |
| Delete Payment Card | ✅ Successful |
| Duplicate Card Creation | ⚠️ Allowed |
| Checkout Process | ✅ Successful |
| Order History | ✅ Successful |
| Order Tracking | ✅ Successful |
| Feedback Submission | ✅ Successful |
| Complaint Submission | ✅ Successful |

---

# Functional Observations

## Basket Operations

All standard shopping basket operations performed correctly and maintained expected application behaviour.

---

## Address Management

Address management functionality operated correctly for creation and deletion.

Duplicate address entries were accepted without validation.

---

## Payment Card Management

Payment cards could be successfully added and deleted.

Duplicate payment card entries were permitted.

No card editing functionality was available in the tested application version.

---

## Checkout Workflow

The complete checkout workflow functioned successfully.

Orders were created correctly and were accessible through both Order History and Order Tracking.

---

## Feedback and Complaint Handling

Feedback and complaint submissions were successfully accepted by the application.

However, submitted entries were not accessible through the user interface during testing, preventing verification of long-term storage behaviour.

---

# Evidence Captured

- Basket management operations
- Address management
- Payment card management
- Checkout workflow
- Order history
- Order tracking
- Feedback submission
- Complaint submission

---

# Conclusion

Functional testing was performed to verify the primary business workflows of the OWASP Juice Shop application.

Core shopping functionality including basket management, checkout, order history, and order tracking operated successfully throughout testing.

Address and payment card management supported creation and deletion operations but permitted duplicate entries. No editing functionality for payment cards was available in the tested application version.

Feedback and complaint submissions were processed successfully; however, submitted entries were not accessible through the user interface for verification.

Overall, the functional assessment confirmed that the primary business features operated as expected while documenting observed application behaviours that may require additional validation in production environments.
