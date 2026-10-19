# Audiodoc Studio — Payment & Balance Testing

**Type:** Functional & Integration Test Design  
**Scope:** Payment for video-processing time and crediting purchased time to the user's balance.  
**Objective:** Verify payment outcomes, consistency between payment status and balance, and protection against incorrect or duplicate crediting.  
**Techniques:** Equivalence Partitioning, Decision Table Testing, State Transition Testing, Risk-Based Test Selection.  
**Environment:** Audiodoc Studio with an integrated Payment Provider Sandbox.  
**Status:** Test Design Sample — designed scenarios only; no execution results claimed.

## Business Rules Under Test

1. Purchased processing time is credited only after final payment success is confirmed.
2. A declined or definitively canceled payment does not change the balance.
3. A single successful payment results in exactly one balance credit.
4. A payment awaiting confirmation must not result in premature crediting.
5. After crediting, purchased time is available for video processing.

These rules are assumptions for this demonstration test design and must be validated against product requirements before execution.

## TC-PAY-001 — Successful Payment and Balance Crediting

**Priority:** High  
**Type:** Positive / End-to-End

**Objective:** Verify that a successful card payment credits the purchased processing time accurately and exactly once.

**Preconditions:**

- The user is signed in.
- The initial available-time balance is recorded.
- The application is connected to a payment sandbox.
- A test-card scenario for successful payment is available.

**Steps:**

1. Open the time-purchase section.
2. Select a processing-time package.
3. Verify that the total price matches the selected tariff.
4. Proceed to card payment.
5. Complete a successful payment in the test environment.
6. Wait for final payment confirmation.
7. Inspect the updated available-time balance.
8. Refresh the page and check the balance again.
9. Verify that the purchased time can be used to process a video.

**Expected Result:**

- The payment reaches a final successful status.
- The available-time balance increases by exactly the purchased duration.
- The time is credited once.
- The correct balance persists after refreshing the page.
- Video processing is available when the remaining eligibility conditions are met.

## TC-PAY-002 — Declined Payment

**Priority:** High  
**Type:** Negative / Integration

**Objective:** Verify that a declined payment does not credit processing time.

**Preconditions:**

- The user is signed in.
- The initial balance is recorded.
- The sandbox supports a declined-payment scenario.

**Steps:**

1. Select a processing-time package.
2. Proceed to payment.
3. Trigger the sandbox's declined-payment scenario.
4. Wait for the final unsuccessful payment status.
5. Inspect the payment message.
6. Inspect the user's available-time balance.
7. Refresh the page and recheck the balance.

**Expected Result:**

- The payment does not reach a successful status.
- The user sees an appropriate unsuccessful-payment result.
- The balance remains unchanged.
- No successful time-credit operation is recorded.
- Refreshing the page does not change the result.

## TC-PAY-003 — User Cancels Payment

**Priority:** High  
**Type:** Negative / State Transition

**Objective:** Verify correct handling when a user cancels payment before it is completed.

**Preconditions:**

- The user is signed in.
- The initial balance is recorded.
- The payment sandbox supports cancellation before final completion.

**Steps:**

1. Select a processing-time package.
2. Open the payment page.
3. Cancel payment before confirmation.
4. Return to Audiodoc Studio.
5. Inspect the displayed payment state.
6. Inspect the user's balance.
7. Refresh the page.

**Expected Result:**

- The incomplete payment is not displayed as successfully paid.
- No time is credited once the payment is definitively canceled.
- The balance remains unchanged.
- The user can start another purchase without a spurious credit from the canceled transaction.

## TC-PAY-004 — Prevent Duplicate Balance Crediting

**Priority:** Critical  
**Type:** Integration / Idempotency

**Objective:** Verify that redelivery of the success notification for the same payment does not credit purchased time twice.

**Preconditions:**

- The user is signed in.
- A test payment has been successfully confirmed.
- Purchased time has already been credited once.
- An approved test mechanism can redeliver a payment notification with the same payment identifier.

**Steps:**

1. Record the confirmed payment identifier and balance after the first credit.
2. Redeliver the notification for **the same payment** using the test mechanism, without creating a new charge.
3. Wait for the duplicate notification to be processed.
4. Inspect the user's balance.
5. Inspect the number of balance-credit entries associated with the payment identifier.
6. Refresh the page and verify the balance again.

**Expected Result:**

- The duplicate notification does not create an additional time credit.
- The balance remains unchanged after the repeated event.
- Exactly one balance-credit operation is associated with the successful payment.
- Duplicate delivery does not compromise payment and balance consistency.

## TC-PAY-005 — Delayed Payment Confirmation

**Priority:** High  
**Type:** Integration / State Transition

**Objective:** Verify that processing time is not credited before final confirmation of payment success.

**Preconditions:**

- The user is signed in.
- The initial balance is recorded.
- The sandbox supports a payment awaiting confirmation followed by a successful final outcome.

**Steps:**

1. Select a processing-time package.
2. Proceed to payment.
3. Create a payment awaiting final confirmation.
4. Inspect the purchase state in Audiodoc Studio.
5. Inspect the balance before payment is finalized.
6. Simulate final successful payment confirmation.
7. Wait for the application to process the confirmation.
8. Inspect the transaction status and updated balance.
9. Refresh the page and check again.

**Expected Result:**

- While payment is pending, processing time is not credited.
- Final success is not communicated to the user prematurely.
- After confirmation, the purchased time is credited exactly once.
- Payment state and balance remain consistent after a page refresh.

## Risk Coverage

| Risk | Test Case |
| --- | --- |
| Successful payment does not credit the correct amount of time | TC-PAY-001 |
| Time is credited without successful payment | TC-PAY-002 |
| Canceled transaction changes the balance | TC-PAY-003 |
| A single payment credits time twice | TC-PAY-004 |
| Time is credited before payment confirmation | TC-PAY-005 |

## Suite Acceptance Criteria

- Successful payment credits the correct amount of time exactly once.
- Declined and definitively canceled payments do not change the balance.
- Duplicate confirmation for the same transaction does not cause duplicate credit.
- Processing time is credited only following final payment success.
- The balance remains consistent after a page refresh.
- Payment state and time-credit records are consistent.

[← Portfolio](../README.md)
