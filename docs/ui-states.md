# Remmogo User Interface States

## 1 Purpose and traceability

This document defines the normal and alternative interface states for Remmogo's critical user tasks. It ensures that the wireframes and implementation deliberately support loading, empty, validation, error, unauthorised, unavailable, stale and offline conditions rather than showing only successful screens.

The primary task is a **Customer searching for an available item and sending a rental request**. The secondary task is an **Administrator generating a semester rental report as a PDF**. Owner confirmation states are included because they determine whether a rental progresses safely through payment, handover and return.

The states trace to:

- FR-01 and FR-02: registration, sign in and sign out;
- FR-03 to FR-07 and FR-11: listing, searching and displaying available quantity;
- FR-08 and FR-09: payment and return confirmation by both parties;
- FR-10 and BR-07: ratings after completed rentals;
- FR-12 and QS-04: Customer-Owner chat;
- FR-13, BR-08 and QS-05: Administrator reporting;
- BR-04: an item may be rented only when quantity is available;
- QS-02: search results should appear within three seconds;
- QS-03: a valid rental transaction should be stored within two seconds;
- QS-06: overdue status and notifications must be issued after the scheduled check;
- QS-07: the system must recover without corrupting pending transactions.

## 2 Shared interface-state rules

These rules apply to every Remmogo screen.

1. Every asynchronous action must show a visible loading state within 100 ms.
2. Loading indicators must include text such as **Searching items** or **Generating report**; animation alone is not sufficient.
3. A button that submits a request is disabled after the first activation until the request succeeds, fails or times out.
4. The system must preserve entered form values after validation errors or recoverable service errors.
5. Error messages must state what happened, what remains safe and what the user can do next.
6. Status must never be communicated using colour alone. Each status uses visible text and, where helpful, an icon.
7. The interface must not show an action that the current role cannot perform. If a protected URL is opened directly, the system shows an unauthorised state rather than an incomplete screen.
8. When displayed data may be stale, the interface shows when it was last updated and provides a refresh action.
9. Destructive or irreversible actions require confirmation and clearly name the affected item, rental or account.
10. Successful state changes are announced visually and through an accessible live region without moving keyboard focus unexpectedly.

## 3 State presentation pattern

Each alternative state should include four parts:

| Part | Requirement |
|---|---|
| State heading | Short description such as **No available items** or **Report generation failed**. |
| Explanation | Plain-language reason without exposing stack traces, database details or internal service names. |
| Primary action | One clear next step, such as **Try again**, **Change filters** or **Sign in**. |
| Safe-state statement | Where relevant, confirm that no duplicate request, payment confirmation or report was created. |

## 4 Customer task states

### Task

The Customer signs in, searches for an available item, reviews its details, selects a rental period and quantity, and sends a rental request.

### Success criteria

- Only available, non-reserved listings are displayed.
- The Customer can understand the item, price, condition, quantity and Owner information before requesting it.
- A valid request is stored once and receives a visible reference and status.
- If availability changes during the task, the Customer receives a clear conflict message and no invalid rental is created.

### 4.1 Search and available-items states

| State | Trigger | Interface treatment | Available actions | Accessibility and integrity notes |
|---|---|---|---|---|
| Initial | Customer opens the storefront with no query. | Show search field, category filter and currently available items. Search field has a persistent label: **Search by item name**. | Search, filter, open listing. | Results region has a heading. Do not use placeholder text as the only label. |
| Loading | Search query or filter changes. | Keep the previous results visible but visually mark them as updating. Show **Searching available items...** and skeleton rows/cards. | Cancel or change query; prevent repeated identical requests. | Loading text uses `aria-live="polite"`; skeletons are hidden from assistive technology. |
| Results | Matching available listings are returned. | Show item name, category, condition, price, available quantity and **View details** action. | Sort, filter, open item. | Result count is announced. Cards follow a logical keyboard order. |
| Empty query result | Search succeeds but nothing matches. | Show **No available items match your search**. Retain query and filters. | Clear filters; search again; browse all available items. | This is not presented as an error. |
| Empty storefront | No listings are currently available. | Show **No items are available right now** with a brief explanation that reserved and unavailable items are hidden. | Refresh; return later. | Do not display unavailable items as if they can be requested. |
| Search error | Search service fails. | Show **We could not load available items** and confirm that no action was submitted. Keep the query. | Try again; clear filters; return to dashboard. | Move focus to the error summary only after a submitted search; retain the visible field label. |
| Offline | Browser loses network connectivity. | Show a persistent **You are offline** banner. Previously loaded results may remain visible with **Last updated [time]** but rental actions are disabled. | Retry connection; view previously loaded details. | Clearly identify cached information as possibly stale. |
| Session expired | Authentication expires while browsing. | Show **Your session has expired**. Preserve the intended destination and non-sensitive search state. | Sign in. | After sign-in, return to the search or item page. Do not preserve passwords or tokens in page state. |

### 4.2 Item-details and rental-request states

| State | Trigger | Interface treatment | Available actions | Accessibility and integrity notes |
|---|---|---|---|---|
| Available | Listing is active with `availableQuantity > 0`. | Show item details, unit price, available quantity, Owner, date fields, quantity field and **Send rental request**. | Enter dates and quantity; send request; chat with Owner. | Price and quantity use text labels, not icons alone. |
| Loading details | Customer opens an item. | Show **Loading item details...** and a stable page structure. | Back to results. | Page title updates when content arrives. |
| Validation error | Required date/quantity is missing or invalid. | Show an error summary and a specific message beside each invalid field. Example: **Quantity must be between 1 and 2**. | Correct fields; resubmit. | Use `aria-describedby`; do not clear valid values; focus the error summary, then allow links to fields. |
| Submitting | Customer sends a valid request. | Button changes to **Sending request...** and is disabled. Other rental fields become read-only until the outcome is known. | Cancel only if the API supports cancellation before storage. | Prevent double activation; send one idempotency key. |
| Request accepted | Rental is stored successfully. | Show **Rental request sent** with rental reference, item, dates, quantity and status `REQUESTED`. | View rental; message Owner; return to storefront. | Announce success using a live region; do not rely on a green colour alone. |
| Availability conflict | Another Customer reserves the requested quantity before submission completes. | Show **This item is no longer available in the quantity requested**. Display the latest quantity and keep dates. | Reduce quantity; refresh item; return to results. | The message states that no rental was created and quantity was not charged or reduced for this attempt. |
| Listing unavailable | Owner removes/deactivates the listing or available quantity reaches zero. | Replace request form with **This item is currently unavailable**. | Return to results; refresh. | Request action is removed, not merely greyed out without explanation. |
| Stale details | Item data changes while the form is open. | Show **Item details changed since you opened this page** and highlight changed price, quantity or status using text. | Review changes; accept and continue; cancel. | Require explicit review if the agreed price changed. |
| Submission timeout | No response arrives within the configured period. | Show **We could not confirm whether your request was received**. Do not invite the user to submit a new request immediately. | Check My Rentals; retry status check. | Reconcile using the request/idempotency key so retry cannot create a duplicate rental. |
| General submission error | Request is rejected for a recoverable internal error. | Show **Rental request could not be sent** and retain the form. | Try again; contact support if repeated. | State that no confirmed request exists unless reconciliation proves otherwise. |
| Unauthorised | A signed-out user or non-Student opens the request action. | Show **Sign in with your UB account to rent this item** or **You do not have permission to perform this action**. | Sign in; return to item. | Use HTTP 401 for unauthenticated and 403 for authenticated-but-forbidden outcomes. |

## 5 Owner task states

### Task

The Owner reviews an incoming rental request, confirms payment and handover, and later confirms the returned item.

| State | Trigger | Interface treatment | Available actions | Integrity notes |
|---|---|---|---|---|
| No pending requests | Owner has no requests requiring action. | Show **No rental requests need your attention**. | View all rentals; manage listings. | Empty state is not an error. |
| Request awaiting decision | Rental status is `REQUESTED`. | Show Customer, item, quantity, dates and request time. | Accept; reject; message Customer. | Recheck availability in the acceptance transaction. |
| Decision submitting | Owner accepts or rejects. | Disable decision buttons and show **Saving decision...**. | None until outcome. | Prevent duplicate decisions. |
| Request already changed | Customer cancelled, another action completed or stale screen submitted. | Show the current status and **This request has already changed**. | Refresh; open current rental. | Do not overwrite a newer state. |
| Waiting for Customer payment confirmation | Owner has not yet received the Customer's confirmation. | Show **Waiting for Customer confirmation** and the rental status. | Message Customer. | Remmogo records confirmation only; it does not process money. |
| Owner confirmation required | Customer has confirmed payment. | Show **Confirm payment received** with rental reference and amount. | Confirm; message Customer. | Confirmation action must identify the confirming party. |
| Confirmation saved | Owner confirmation succeeds. | Show both parties' confirmation states and the next lifecycle action. | Confirm handover when permitted. | Only one confirmation per party per rental. |
| Return confirmation pending | One party has confirmed the return. | Show who has confirmed and **Waiting for [Customer/Owner]**. | Confirm return if eligible; message other party. | Item quantity is restored only according to the agreed lifecycle rule, not on repeated clicks. |
| Overdue | Due date passes without completed return confirmation. | Show **Rental overdue** with due date and required action. | Message party; confirm return; report issue. | Status text and warning icon are used together; overdue is not colour-only. |

## 6 Administrator PDF-report states

### Task

The Administrator selects a reporting period and rental statuses, generates a semester rental report and downloads the completed PDF.

### Success criteria

- Only an Administrator can generate or download the report.
- Validation prevents invalid reporting periods.
- One request creates at most one `REPORT_EXPORT` record and one external PDF document.
- A report containing more than 500 records is normally ready within ten seconds under QS-05.
- Service failures give stable messages and safe retry behaviour.

| State | Trigger | Interface treatment | Available actions | API/error mapping and accessibility notes |
|---|---|---|---|---|
| Initial | Administrator opens Generate Report. | Show report type, labelled start/end dates, status filters and **Generate PDF**. | Set filters; generate. | Explain that the report may contain personal information and is for administrative use. |
| Validation error | Missing/invalid dates or unsupported filters. | Show error summary plus field-level messages. Preserve all valid selections. | Correct request. | `400 INVALID_REPORT_REQUEST`; error links move focus to fields. |
| Generating | Valid request submitted. | Disable **Generate PDF** and show **Generating report...** with elapsed time. | Cancel view; continue using other admin pages if processing is asynchronous. | Use `aria-live="polite"`; create and retain one idempotency key. |
| Completed | PDF service returns successfully. | Show **Report ready**, record count, period, completion time and **Download PDF**. | Download; generate another report. | Download route remains authenticated; success is not colour-only. |
| No report data | Valid filters match no rentals. | Show **No rental records match this report**. | Change filters; change period. | `422 NO_REPORT_DATA`; do not create an empty PDF unless the project explicitly chooses that behaviour. |
| Unauthorised | Non-Administrator opens the page or endpoint. | Show **You do not have permission to generate reports**. | Return to dashboard; sign in with an authorised account. | `401 AUTHENTICATION_REQUIRED` or `403 ADMIN_ROLE_REQUIRED`. |
| Service unavailable | PDF rendering dependency is unavailable. | Show **The PDF service is temporarily unavailable** and confirm that rental data was not changed. | Try again with the same request; return later. | `503 PDF_SERVICE_UNAVAILABLE`; retry uses the same idempotency key. |
| Timeout / unknown outcome | Rendering does not respond within timeout. | Show **Report generation is taking longer than expected**. Display a pending status rather than generating a second report. | Check status; retry status check. | `504 PDF_SERVICE_TIMEOUT`; reconcile by correlation/idempotency key before another render. |
| Failed | Export reaches terminal `FAILED` status. | Show **Report generation failed**, correlation/reference ID and retry guidance. | Retry; contact support. | `500 REPORT_EXPORT_FAILED`; do not expose stack traces or external credentials. |
| Duplicate key conflict | Same idempotency key is reused with different filters. | Show **This report request conflicts with an earlier request**. | Start a new report request. | `409 IDEMPOTENCY_KEY_REUSED`; generate a new key only after an intentional new submission. |
| Expired download | Generated file passed its retention/expiry time. | Show **This report download has expired**. | Regenerate report. | Do not link to the external rendering service directly. |
| Offline | Administrator loses connectivity. | Show persistent offline banner and retain selected filters locally for the session. Disable generation. | Retry connection. | Do not cache completed PDFs in shared browser storage. |

## 7 Chat states

| State | Interface treatment | Required behaviour |
|---|---|---|
| Loading conversation | Show **Loading messages...** while retaining the conversation header and rental reference. | Messages are presented oldest-to-newest with a logical reading order. |
| Empty conversation | Show **No messages yet** and a labelled message field. | The empty state invites the user to start the conversation. |
| Sending | Show the outgoing message as **Sending...** and disable repeated submission of the same message. | Preserve message text until confirmed stored. |
| Sent | Mark the message **Sent**. | QS-04 expects the online recipient to see it within one second. |
| Delivery failed | Mark only the affected message **Not sent** and retain its text. | Provide **Retry** and prevent duplicate messages using a client message ID. |
| Unauthorised participant | Show **You cannot access this conversation**. | Only the Customer, Owner and authorised Administrator may access relevant communication. |
| Offline | Show **Messages will not send while offline**. | Do not falsely mark queued messages as sent. |

## 8 Rating states

| State | Trigger | Interface treatment | Available actions |
|---|---|---|---|
| Not eligible | Rental is not completed or both return confirmations are incomplete. | Explain **Ratings are available after the rental is completed and the return is confirmed**. | View rental. |
| Eligible | Rental is completed and current Student has not rated the other party. | Show labelled 1-5 rating choices and optional comment. | Submit rating. |
| Validation error | No score or invalid score. | Show **Choose a rating from 1 to 5** beside the control and in the summary. | Correct and submit. |
| Submitting | Rating is being stored. | Disable submit and show **Submitting rating...**. | Wait. |
| Submitted | Rating succeeds. | Show **Rating submitted** and the recorded score/comment. | Return to rental. |
| Already rated | A duplicate rating is attempted. | Show the existing rating and explain that one rating per party per rental is allowed. | View rating. |

## 9 Account and authentication states

| State | Interface treatment | Requirement |
|---|---|---|
| Registration validation | Show field-level errors for invalid UB email, missing name or password requirements. | FR-01 and BR-01; retain non-sensitive valid fields. |
| Email already registered | Show **An account already exists for this email**. | Offer sign-in or password-recovery path without exposing unrelated account details. |
| Sign-in loading | Disable sign-in button and show **Signing in...**. | Prevent duplicate requests. |
| Invalid credentials | Show **Email or password is incorrect**. | Do not reveal which credential was wrong. |
| Account suspended/deactivated | Show **This account is not currently active** and a support path. | Do not expose administrative notes. |
| Session expired | Explain that the session ended and request sign-in again. | Return to the safe intended page after authentication. |

## 10 Responsive and accessible state requirements

- All state headings, explanations and actions remain readable at 200% browser zoom without horizontal scrolling for normal page content.
- At widths below 768 px, tables or card grids become stacked labelled blocks; information is not conveyed by column position alone.
- Keyboard focus order follows the visible task order. Dialogs trap focus only while open and return focus to the action that opened them.
- Every interactive element has a visible focus indicator with at least a 3:1 contrast ratio against adjacent colours.
- Text and essential icons meet WCAG AA contrast requirements.
- Inputs use persistent text labels. Required fields are identified in text, and instructions appear before submission.
- Errors are associated programmatically with their fields and summarised at the top of the form.
- Status badges contain readable text such as `REQUESTED`, `ACTIVE`, `OVERDUE` or `COMPLETED`; colour is supplementary.
- Touch targets are at least 44 by 44 CSS pixels where practical.
- Motion is limited and respects `prefers-reduced-motion`.
- Loading, success and error updates use appropriate live regions without repeatedly interrupting screen-reader users.

## 11 State-transition safeguards

| User action | UI safeguard | Data safeguard |
|---|---|---|
| Send rental request | Disable repeated submission and show progress. | Idempotency key plus atomic availability check. |
| Accept rental request | Revalidate current status and availability. | Transactional reservation with row lock or optimistic version check. |
| Confirm payment/return | Show which party is confirming and current confirmation state. | Unique `(rental_id, confirmed_by_student_id)` constraint. |
| Send chat message | Give each pending message a client ID. | Retry returns/reuses the same stored message rather than duplicating it. |
| Submit rating | Disable after submission and display recorded result. | Unique `(rental_id, author_student_id)` constraint. |
| Generate PDF report | Disable repeated generation and retain request status. | Unique idempotency key and reconciliation after timeout. |

## 12 Verification checklist

-  Search loading state appears immediately and results arrive within QS-02's three-second target under the defined load.
-  An empty search is distinguishable from a search failure.
-  A Customer cannot request a listing with zero available quantity.
-  Two Customers requesting the final unit produce one success and one availability-conflict state.
-  Validation errors preserve valid form data and move focus to an error summary.
-  A timed-out rental request can be reconciled without creating a duplicate.
-  A suspended or unauthorised user cannot reach protected actions through a direct URL.
-  Payment and return confirmations show the confirming party and cannot be duplicated.
-  Overdue status uses text and an icon, not colour alone.
-  Chat send failure preserves the unsent message and allows safe retry.
-  A rating cannot be submitted before the rental is completed.
-  PDF generation supports loading, no-data, unauthorised, unavailable, timeout, failed, completed and expired states.
-  Retrying a timed-out PDF request with the same idempotency key produces one export.
-  Every interactive state can be reached and exited using only the keyboard.
-  Screen-reader announcements identify loading, success and error outcomes without excessive repetition.

## 13 Revision record

| Revision | Trigger or observation | Change made | Verification |
|---|---|---|---|
| 1.0 | Initial Lab 10 alternative-state design. | Added normal, loading, empty, validation, error, unauthorised, unavailable, stale and offline states for Customer, Owner and Administrator tasks. | Review against wireframes and run the three-person task-based usability check. |
| 1.1 | To be completed after usability testing. | Record the actual interface change made following an observed user difficulty. | Repeat the affected task with at least one participant and record whether the difficulty is resolved. |

