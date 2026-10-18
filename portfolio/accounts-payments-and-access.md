# Accounts, payments and access

**Reference test design — not an execution report.** These cases describe how to check supporting Audiodoc Studio journeys. They do not claim that I executed this suite, discovered a particular defect, or implemented the features. Actual results belong in a dated execution report tied to a build.

[Profile](../README.md) · [Product coverage](product-coverage.md) · [Regression and release](regression-and-release.md)

## Scope and preparation

Use a non-production environment, dedicated test accounts and the payment provider's sandbox. Prepare an owner account with disposable documents and projects, a second signed-in account, a new account, and a controlled test mailbox. Use synthetic transcripts. Never charge a real card, contact a real customer or delete production documents for these cases.

Record the build, environment, browser, account roles, relevant configuration and starting balance. Obtain approved sandbox fixtures for successful, failed and delayed payments, discounts, referrals, legacy accounts and consent-required accounts. If a fixture or provider behavior cannot be controlled, mark that case **Blocked** rather than improvising a real transaction.

The suite combines source-observed behavior with expected business invariants. Source inspection establishes available controls and intended transitions; it does not prove that the deployed identity provider, database functions, email delivery or payment callbacks work. Confirm environment-specific requirements before execution. A pending asynchronous operation is not a pass or a failure until its agreed observation window has elapsed.

**Priority:** P0 protects account access, document permissions and financial correctness. P1 covers supporting workflows and state consistency. Priority determines execution order; it is not a measured defect severity.

## Authentication and account recovery

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| AUTH-01 | P0 | Sign in with a valid email/password account; open the document workspace, then reload. | The user reaches the workspace, remains signed in after reload and sees the correct account's documents and balance. No stale redirect returns the user to sign-in. |
| AUTH-02 | P1 | Try an incorrect password, an invalid email format and an empty required field. Then correct the input. | Invalid input cannot create an authenticated session. Feedback allows correction, and a subsequent valid attempt succeeds without getting stuck in a loading state. |
| AUTH-03 | P0 | Complete Google sign-in with the prepared account. For an approved legacy-account fixture, follow its supported recovery/sign-in path using the same email. | Google authentication returns to the workspace. The legacy fixture retains its existing documents and balance under the intended account identity; a new empty account must not silently replace it. |
| AUTH-04 | P0 | Request a password reset through the test mailbox. Test a valid link, mismatched confirmation, a missing token and a provider-issued expired/used token. | A valid reset changes the password and permits sign-in with the new password. Invalid input/token cannot change it. Recovery email content is appropriate to the account type; a request acknowledgement alone does not establish delivery. |
| AUTH-05 | P1 | From account settings, change the password with an incorrect current password, then with valid credentials. Sign in again in a fresh session. | An invalid change is rejected. A successful change clears the form, reports success and allows the new password to authenticate. Check Google-only accounts against their documented password policy. |
| AUTH-06 | P0 | Sign out and revisit a protected workspace URL. Also open it in a clean unsigned-in browser context. | Protected account data is unavailable until authentication. Signing in as the second test account does not expose the first account's private documents or balance. |

## Consent and onboarding

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| CON-01 | P1 | On signup, toggle the required offer and personal-data checkboxes independently; leave mailing unchecked. Test both email and Google signup controls. | Signup remains disabled until both required choices are selected. Optional mailing consent is not required to proceed. |
| CON-02 | P1 | Toggle Select all, then clear one individual checkbox; repeat with only mailing cleared. | Individual choices and the Select all indicator stay consistent. Clearing mailing alone does not disable signup; clearing a required choice does. |
| CON-03 | P1 | Enter name/email, open a linked legal document, then return to signup in the same session. | The legal document is readable and the name/email draft is restored. A password is not restored from that draft. Consent choices must reflect the actual current selection. |
| CON-04 | P1 | Complete a Google signup after selecting consents. Separately use a consent-required existing account; accept its prompt, navigate and reload. Simulate a consent-status read failure only in the test environment. | Successful signup carries the selected consents through the redirect without an unnecessary repeat prompt. Successful existing-user acceptance dismisses the gate. Legal pages remain accessible. The current status-read fallback is fail-open: it must not lock out the user, and absence of the prompt during failure is not evidence of recorded consent. |
| CON-05 | P1 | In separate clean browser contexts, accept and decline the cookie banner; navigate and reload. Check desktop and narrow layout. | The chosen state persists according to the agreed browser-storage behavior. The banner offers both choices, remains usable and does not obscure essential controls. If analytics behavior is in scope, verify it separately against the consent policy. |

## Payments and account history

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| PAY-01 | P0 | Select valid top-ups at 1 and 3000 minutes. Try empty, zero, negative, fractional and above-maximum values. Compare minute/hour input modes. | Only a valid positive whole-minute purchase within the supported range reaches payment initiation. The selected duration remains unambiguous across display modes and matches the credited seconds. Record any UI rounding before comparing amounts. |
| PAY-02 | P0 | Compare the displayed price and sandbox checkout amount immediately below, at and above each configured volume threshold. | The correct tier applies to each selected quantity. The amount shown before payment agrees with the amount submitted to the provider and the eventual transaction record. Use the build's approved pricing fixture rather than a historical rate. |
| PAY-03 | P0 | Use discount fixtures below/at the minimum purchase, before/after expiry and with zero/remaining uses; include two eligible discounts. | Only an eligible discount changes the charge. The highest applicable percentage is selected; equal percentages prefer a time-limited discount. Confirmation consumes a limited use once. Inspect displayed and charged amounts separately. |
| PAY-04 | P1 | Select amounts below/at/above the configured payment-method threshold, including a discount that crosses it. Start each enabled sandbox method. | The available methods and restriction message match the approved rules for the actual charge. Payment initiation requires an email for the receipt. Record a discrepancy between UI eligibility and provider eligibility rather than assuming one proves the other. |
| PAY-05 | P0 | Complete a successful sandbox payment. Cover redirect return and an SBP-style flow completed on another device/context without a browser redirect. Observe the pending state and confirmation. | The UI can remain pending while confirmation is outstanding. Once the payment is confirmed, the balance increases by the purchased seconds, the transaction succeeds and the displayed balance refreshes. A success URL alone must not create credit. |
| PAY-06 | P0 | Use declined, cancelled and delayed-confirmation fixtures. Return to the workspace and reload while the payment is pending. | Failed or cancelled payments do not credit the balance. Pending payments remain distinguishable from confirmed ones. The user receives appropriate feedback and can recover after a terminal failure; a timeout is recorded with the provider state still outstanding. |
| PAY-07 | P0 | With an authorized sandbox callback fixture, deliver the same valid confirmation twice; also exercise a wrong-amount or invalid-signature fixture. Compare balance and history before/after. | A valid payment is credited exactly once. Invalid confirmation data must not add balance. This case requires controlled provider/backend evidence; repeated browser clicks alone do not prove callback idempotency. |
| ACC-01 | P1 | View transaction history after a successful top-up and a documented spend. Delete only a disposable owned document, then inspect its transaction entry again. | Amounts, time units, status and associated filename are understandable and consistent. Deleting a document does not erase the financial history; its archived-document status remains distinguishable where presented. |

## Sharing and project organization

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| SHR-01 | P0 | Open one document as owner, signed-out visitor and second account under private, view and edit settings. Attempt viewing and saving changes through the supported UI. | Private access is limited to the owner. Shared view allows an authenticated recipient to read but not edit; shared edit permits the supported editing actions. Sharing does not provide anonymous access. Owner-only actions remain restricted. |
| SHR-02 | P0 | Change sharing from private to edit, decline/accept the warning, then revoke access while the second account has the document open. Attempt another protected action. | Declining the warning preserves the previous setting. Accepted changes persist. Revocation prevents further protected operations by the former recipient; neither view nor edit access allows changing sharing or deleting the document. Already-rendered content is distinct from new authorized access. |
| PROJ-01 | P1 | Create and rename a project with a normal name; exercise empty/whitespace names and a representative long name. Reload and compare the project picker and sidebar. | Valid names persist consistently across surfaces. Invalid names are handled according to the agreed validation rule; the UI must not report a failed creation/rename as successful. Confirm the naming rule before treating a validation difference as a defect. |
| PROJ-02 | P1 | Assign an owned document to a project, move it to another, then unassign it. Reload, filter by each project and repeat as the second account. | Assignment and filters reflect the saved state; unassigned documents remain accessible in the appropriate list. The second account cannot reorganize the owner's private documents. A failed save must not be mistaken for persisted movement. |

## Referral lifecycle

| ID | Priority | Action and data | Expected outcome |
|---|---|---|---|
| REF-01 | P1 | Generate and copy a referral link for a test referrer; use it with a new test account, then deactivate it. Check the displayed registration/payment counters against the approved fixtures. | The link belongs to the intended referrer, copy produces the correct destination and deactivation persists. Counter changes correspond to eligible events; a registration does not imply a completed payment. |
| REF-02 | P0 | For a referred test account, prepare settled purchases totaling below, exactly and above one hour. Include pending/failed purchases and repeat the qualifying completion event. | Only settled purchases qualify toward the one-hour threshold. The eligible referral reward is issued once and recorded consistently; pending/failed payments and duplicate triggers do not create extra credit. Validate the configured reward amount rather than assuming it. |

## Recording results

For each executed case record **Passed / Failed / Blocked / Not run**, actual behavior, build, browser, test-account role and sanitized evidence. For payments, record starting/ending seconds, requested minutes, charged amount, transaction status and a redacted correlation identifier. Keep callback/provider evidence separate from screenshots of UI messages.

Do not include credentials, tokens, real payment details, customer transcripts or personal account information in this repository. Self-service refunds are outside this suite: the current product uses a manual refund process. This design also does not establish legal compliance, penetration-test coverage or successful execution of the deployed backend.
