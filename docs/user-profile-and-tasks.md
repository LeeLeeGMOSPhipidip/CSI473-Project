# RRemogo: User Profile and Task Scenario

Course: CSI473  
Laboratory: 10  
Project: RRemogo Student Rental Marketplace

## 1. User Profile

### Primary User: Student Customer

The primary user for this task is a University of Botswana student who wants to rent an item for a specific period instead of buying it. The student uses RRemogo to find available items, check their details and submit a rental request.

| Attribute | Description |
|---|---|
| User role | Student acting as a customer |
| User group | University of Botswana students |
| Main goal | Find an item and request to rent it |
| Access requirements | A valid UB account and an active sign-in session |
| Main activities | Search for items, view item details, select a rental period and submit a request |
| Possible difficulties | An item may be unavailable, required information may be missing, or the system may fail to process a request |

### User Needs

The customer needs to find items easily, see whether they are available, understand the rental price and select the period for which the item is needed. The system should provide clear feedback after the request is submitted and explain what to do if the request cannot be completed.

## 2. Task Scenario

### Request to Rent an Item

A UB student needs an item for a limited period. The student signs in to RRemogo, searches for the item and checks its details. If the item is available, the student selects the required rental period and submits a request.

The system checks availability and records the request. The customer should receive a clear confirmation of the request status and information about the next steps.

### Preconditions

- The student has a valid UB account.
- The student is signed in to RRemogo.
- The item has been listed on the platform.
- The item is available for the requested period.

### Main Success Flow

| Step | User action | System response |
|---|---|---|
| 1 | The student opens RRemogo. | The system displays the available marketplace functions. |
| 2 | The student searches for an item or browses the listings. | The system displays matching items and their availability. |
| 3 | The student selects an item. | The system displays the item's description, condition, rental price and availability. |
| 4 | The student selects the required rental period. | The system accepts the selected period for validation. |
| 5 | The student submits the rental request. | The system checks whether the item is available for that period. |
| 6 | The request passes validation. | The system records the request and updates the relevant reservation status. |
| 7 | The student views the confirmation. | The system displays the request status and explains the next steps. |

### Postconditions

If the request is accepted, the system records the rental request and reflects the appropriate reservation status. The customer can view the status and follow the next steps in the rental process.

A successful request does not necessarily mean that the rental is fully active. Payment confirmation and handover may still need to be completed according to the agreed rental process.

## 3. Alternative and Error Scenarios

| Situation | System response |
|---|---|
| The item is unavailable. | The system informs the customer that the item cannot be requested for the selected period. |
| The customer does not select a rental period. | The system identifies the missing information and asks the customer to provide it. |
| The submitted information is invalid. | The system explains what needs to be corrected. |
| The request is still processing. | The system provides feedback that the request is being processed and prevents unnecessary repeated submissions. |
| A system error occurs. | The system displays an understandable error message and advises the customer to try again. |
| The customer's session expires. | The system asks the customer to sign in again before continuing. |
| The item becomes unavailable before the request is confirmed. | The system rejects the request and informs the customer that the item's availability has changed. |

## 4. Accessibility Considerations

The rental request process should be accessible and understandable to different users.

- All input fields should have clear labels.
- Users should be able to navigate the interface using a keyboard.
- The current keyboard focus should be visible.
- Error messages should explain the problem and how to correct it.
- Important information should not be communicated through colour alone.
- Text should be readable on different screen sizes.
- Loading indicators and confirmation messages should make the system's current status clear.
- Buttons should use descriptive labels so that users understand what each action does.

## 5. Requirements Traceability

| Project requirement | How it is addressed |
|---|---|
| Students must sign in using valid UB accounts. | Sign-in is included as a precondition. |
| Customers must be able to find items. | Searching and browsing are included in the task flow. |
| Customers must be able to view item details. | The item selection step displays the relevant listing information. |
| Customers must be able to request rentals. | The main success flow describes submitting a request. |
| The system must check availability. | Availability is checked before the request is accepted. |
| The system must provide feedback. | The task includes confirmation, validation and error responses. |
| Rental status must be recorded. | The system records the request and reflects the appropriate reservation status. |

## 6. Design Rationale

The rental request was selected because it is one of the main tasks performed by a customer on RRemogo. The process covers searching for an item, checking its availability, providing the rental period and submitting a request.

The task also considers situations where the process cannot be completed. These include unavailable items, missing information and system errors. Handling these situations helps users understand what happened and what action they can take next.

The design separates submitting a rental request from completing the entire rental transaction. This reflects the project's process, in which payment confirmation and handover may occur after the request has been recorded.

## 7. Usability Evaluation Plan

A usability check will be conducted with three participants. Each participant will be asked to find an available item and submit a rental request.

The evaluation will record whether the participant completes the task, any difficulties encountered, the system's response and any changes made to the design.

| Item | What will be recorded |
|---|---|
| Task | Find an available item and submit a rental request. |
| Observation | The participant's actions, confusion or errors. |
| Result | Whether the participant completes the task successfully. |
| Revision | Any interface changes made in response to the findings. |

The usability results and revisions will be documented after the participants have completed the task.
