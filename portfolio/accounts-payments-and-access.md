# Accounts, payments and access

**Reference test design — not an execution report.** Record actual results against a dated build.

[Profile](../README.md) · [Product coverage](product-coverage.md) · [Regression and release](regression-and-release.md)

## Preparation

Use a test environment and payment sandbox. Prepare an owner with disposable documents/projects, a second account, a new account and a test mailbox. Use synthetic transcripts; no real charges, customer contact or production deletions.

Record the build, environment, browser, account roles, settings and starting balance. Prepare successful, failed and delayed payments, discounts, referrals, legacy accounts and accounts requiring consent. Mark unavailable fixtures **Blocked**.

Expected outcomes below combine behavior found in source code with business requirements. Confirm environment-specific rules before testing. Check deployed services directly: code alone cannot verify email delivery, account migration or payment confirmation. Agree how long to observe pending operations before recording a timeout.

**Priority:** P0 — account access, permissions and financial correctness; P1 — supporting workflows and state consistency. Priority sets test order, not defect severity.

## Authentication and account recovery

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| AUTH-01 | P0 | Sign in with valid email/password credentials, open the workspace and reload. | The session survives reload and shows the correct documents and balance. No stale redirect returns the user to sign-in. |
| AUTH-02 | P1 | Try an incorrect password, invalid email format and empty required field; then correct them. | Invalid input is rejected with useful feedback. Valid input succeeds without a stuck loading state. |
| AUTH-03 | P0 | Sign in through Google. Separately, recover/sign in to a prepared legacy account using its original email. | Google returns to the workspace. The legacy account retains its documents and balance rather than becoming a new empty account. |
| AUTH-04 | P0 | Request a reset email. Test a valid link, mismatched passwords, missing token and provider-issued expired/used token. | Only a valid reset changes the password. The new password works. Check actual email delivery and account-appropriate instructions, not just the request acknowledgement. |
| AUTH-05 | P1 | Change the password with an incorrect current password, then valid credentials. Sign in in a fresh session. | Invalid changes are rejected. Success clears the form, shows confirmation and permits login with the new password. Apply the documented policy to Google-only accounts. |
| AUTH-06 | P0 | Sign out and revisit a protected URL; repeat in a clean browser context, then sign in as the second account. | Unsigned-in users cannot access protected data. The second account cannot see the first account's private documents or balance. |

## Consent and onboarding

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| CON-01 | P1 | Toggle required offer/personal-data consents separately; leave mailing unchecked. Check email and Google signup. | Both required consents enable signup. Mailing consent remains optional. |
| CON-02 | P1 | Toggle Select all, then clear an individual checkbox; repeat with only mailing cleared. | Select all reflects individual choices. Clearing mailing leaves signup enabled; clearing a required consent disables it. |
| CON-03 | P1 | Enter name/email, open a legal document and return within the same session. | The document is readable. Name/email return from the draft; password does not. Consent checkboxes reflect the current selection. |
| CON-04 | P1 | Complete Google signup with consents. Separately accept an existing account's consent prompt, navigate and reload. Simulate a status-read failure. | Signup carries consents through the redirect without a repeat prompt. Acceptance dismisses the existing-user prompt; legal pages stay accessible. A status-read failure leaves access open, but does not prove consent was recorded. |
| CON-05 | P1 | Accept and decline cookies in separate clean contexts; navigate and reload. Check desktop and narrow layouts. | Choices persist under the agreed storage rules. Both buttons remain usable without covering essential controls. If analytics is in scope, check it separately against the consent policy. |

## Payments and account history

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| PAY-01 | P0 | Test 1 and 3000 minutes; empty, zero, negative, fractional and over-limit values. Switch minutes/hours. | Payment accepts whole minutes within 1–3000. Displayed duration and credited seconds agree. Record UI rounding before comparing amounts. |
| PAY-02 | P0 | Compare displayed and checkout prices below, at and above each volume threshold, using the build's approved rates. | The correct tier applies. Displayed price, provider charge and transaction record agree. |
| PAY-03 | P0 | Test discounts below/at minimum purchase, before/after expiry, with zero/remaining uses and with two eligible discounts. | The highest eligible percentage applies; ties prefer time-limited discounts. Confirmation consumes one limited use once. Compare displayed and charged amounts. |
| PAY-04 | P1 | Test below/at/above payment-method thresholds, including a discount crossing a threshold. Start each enabled sandbox method. | Methods and restriction messages follow the rules for the actual charge. Receipt email is required. Record any UI/provider eligibility mismatch. |
| PAY-05 | P0 | Complete payments through redirect return and SBP on another device/context without a redirect. Observe pending and confirmed states. | Before confirmation, treat the payment as pending. Confirmation credits the purchased seconds, marks the transaction successful and refreshes balance. A success URL alone cannot credit it. |
| PAY-06 | P0 | Test decline, cancellation and delayed confirmation. Return to the workspace and reload while pending. | Failed/cancelled payments add no balance. Pending remains distinct from confirmed. Failure feedback allows recovery; record timeouts with the provider status still outstanding. |
| PAY-07 | P0 | Using approved sandbox callbacks, send a valid confirmation twice; also test wrong amount and invalid signature. Compare balance/history. | Valid credit occurs once; invalid confirmations add nothing. Verify with provider/backend records: repeated browser clicks do not test callback idempotency. |
| ACC-01 | P1 | View history after top-up and spending. Delete an owned disposable document and recheck its transaction. | Amounts, units, status and filename agree. Deletion preserves transaction history and its archived-document indication where shown. |

## Sharing and projects

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| SHR-01 | P0 | Try reading/saving as owner, signed-out visitor and second account under private/view/edit settings. | Private access is owner-only. Signed-in recipients can read in view mode and edit in edit mode. No anonymous access; owner-only actions stay restricted. |
| SHR-02 | P0 | Switch private → edit; decline then accept the warning. Revoke sharing while the second account has the document open; attempt another protected action. | Declining preserves the setting; accepting saves it. Revocation blocks new protected operations. Recipients cannot change sharing or delete. Already-rendered content is separate from fresh access. |
| PROJ-01 | P1 | Create/rename with normal, empty, whitespace and long names. Reload and compare picker/sidebar. | Valid names persist consistently; failed saves are not shown as successful. Confirm naming rules before classifying validation differences as defects. |
| PROJ-02 | P1 | Assign, move and unassign an owned document. Reload, check each project's filter and repeat as the second account. | Filters reflect saved assignments; unassigned documents remain listed. The second account cannot reorganize private documents. Failed saves must not look persisted. |

## Referrals

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| REF-01 | P1 | Generate/copy a referral link, use it with a new test account, then deactivate it. Check registration/payment counters. | The copied link identifies the correct referrer; deactivation persists. Counters match qualifying events, with registration separate from payment. |
| REF-02 | P0 | Test settled purchases totaling below/exactly/above one hour for an invited account. Include pending/failed payments and repeat the qualifying event. | Only settled purchases count toward the hour. The configured reward is credited and recorded once; pending/failed payments or duplicate events add no extra credit. |

## Recording results

Record **Passed / Failed / Blocked / Not run**, actual behavior, build, browser, role and sanitized evidence. For payments include starting/ending seconds, purchased minutes, charge, status and a redacted correlation ID. Keep provider/callback evidence separate from UI screenshots.

Remove credentials, tokens, payment details and customer data from evidence. Refunds use a manual process and are outside this suite, as are legal compliance assessment and penetration testing.
