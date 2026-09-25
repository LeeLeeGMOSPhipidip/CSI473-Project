# Quality to Architecture

## 1. Purpose

This document shows how the quality requirements identified for Remmogo influence the architectural design. It identifies the main architecture drivers, the design obligations created by those drivers, and the components responsible for addressing them.

## 2. Architecture Drivers

The following architecture drivers were identified from the functional requirements, quality scenarios, acceptance criteria, and project constraints.

| Driver                                           | Description                                                                                                              | Source                                                 |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ |
| AD-01: Availability Integrity                    | The system must prevent conflicting rental requests when several Customers request the same limited quantity of an item. | FR-07, FR-11, duplicate-booking acceptance criterion   |
| AD-02: Trusted Student Access                    | The system must restrict registration and protected actions to users with valid UB email accounts.                       | FR-01, FR-02, project scope, concurrent login scenario |
| AD-03: Timely Interaction and Lifecycle Response | The system must provide acceptable response times for searching, rental transactions, chat, and overdue processing.      | Quality Scenarios 2, 3, 4 and 6                        |

The architecture therefore needs to keep availability management, authentication and authorization, rental processing, persistence, and notifications sufficiently separated so that these concerns can be developed and tested independently.

## 3. Quality Requirements and Architectural Obligations

| Quality Requirement      | Scenario / Requirement                                                     | Target                                                         | Architectural Obligation                                                                                            |
| ------------------------ | -------------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Availability Integrity   | Multiple Customers request an item with limited quantity at the same time. | Prevent duplicate or conflicting bookings.                     | Rental creation and item availability updates must be controlled together.                                          |
| Authentication           | Multiple students attempt to log in simultaneously.                        | 50 students authenticate within 5 seconds.                     | Authentication must be separated from rental processing and support concurrent login requests.                      |
| Search Performance       | Customer searches a catalogue containing more than 1,000 items.            | Results within 3 seconds.                                      | Search and item retrieval must be separated from other rental processing.                                           |
| Transaction Performance  | A rental transaction is created or confirmed.                              | Database operations within 2 seconds.                          | Rental transactions must use efficient database operations and controlled state updates.                            |
| Chat Responsiveness      | Two Rental participants exchange messages while online.                    | Message visible within 3 seconds.                              | Chat functionality must have its own communication responsibilities rather than being mixed with rental processing. |
| Administrative Reporting | Administrator generates a report containing more than 500 rental records.  | Report available within 10 seconds.                            | Reporting must be separated from normal rental transaction processing.                                              |
| Overdue Processing       | A Rental passes its due date.                                              | Rental marked Overdue and both users notified within 1 minute. | A scheduled process must check rental due dates and trigger notifications.                                          |

The response-time targets and scenarios are based on the quality scenarios defined in Phase 1.

## 4. Quality Requirements and Components

The Remmogo architecture uses a three-layer structure consisting of Presentation, Application, and Data layers.

| Quality Concern          | Presentation Layer                      | Application Layer                                    | Data Layer                     |
| ------------------------ | --------------------------------------- | ---------------------------------------------------- | ------------------------------ |
| Availability Integrity   | Rental request interface                | Rental, ItemListing and availability rules           | Rental and ItemListing records |
| Trusted Student Access   | Registration and login screens          | Student and Account authentication and authorization | User and account information   |
| Search Performance       | Item browsing and search interface      | Item listing and search operations                   | Item listing data              |
| Transaction Performance  | Rental request and confirmation screens | Rental transaction management                        | Rental records                 |
| Chat Responsiveness      | Chat interface                          | Conversation and Message handling                    | Chat history                   |
| Administrative Reporting | Administrator report screens            | Report generation                                    | Rental and administrative data |
| Overdue Processing       | Rental status and notification display  | Rental lifecycle and notification handling           | Rental due dates and status    |

## 5. Architectural Responsibilities

| Component / Responsibility | Main Responsibility                                                       | Quality Concern Addressed                                         |
| -------------------------- | ------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Student / Account          | Registration, authentication, authorization and account management.       | Trusted student access                                            |
| Owner                      | Listing items, confirming payment received, handover and return actions.  | Availability integrity, transaction lifecycle                     |
| Customer                   | Searching for items, initiating rentals, confirming payment and return.   | Availability integrity, transaction lifecycle                     |
| ItemListing                | Stores item information and maintains available quantity.                 | Availability integrity, search performance                        |
| Category                   | Classifies and filters item listings.                                     | Search performance                                                |
| Rental                     | Manages the rental lifecycle, due dates, status changes and rental rules. | Availability integrity, transaction performance, overdue handling |
| Rating                     | Records ratings after completed rentals.                                  | Functional correctness                                            |
| Conversation / Message     | Stores and delivers communication between authorized participants.        | Chat responsiveness and access control                            |
| Administrator / Report     | Manages reports, disputes and administrative reporting.                   | Reporting performance and administration                          |

These responsibilities follow the component and domain responsibilities defined in the Phase 1 design.

## 6. Availability Integrity

Availability integrity is a major architectural concern because an item may have a limited quantity and multiple Customers may attempt to rent it.

The Rental lifecycle includes a `Reserved` state. When an Owner accepts a rental request, the requested quantity is reserved and is no longer treated as available to other Customers. If the transaction is cancelled or does not proceed, the reservation is released and the quantity becomes available again.

| Situation                                               | Required System Behaviour                              |
| ------------------------------------------------------- | ------------------------------------------------------ |
| Item has available quantity                             | Customer may request the item.                         |
| Owner accepts rental request                            | Requested quantity is reserved.                        |
| Another Customer requests the same unavailable quantity | System prevents a conflicting booking.                 |
| Payment confirmation is not received within 48 hours    | Rental is cancelled and reserved quantity is released. |
| Rental is cancelled or a no-show occurs                 | Reserved quantity is restored.                         |
| Rental is completed                                     | Item availability is updated accordingly.              |

The `Reserved` state was introduced to address the availability gap identified during the previous design review.

## 7. Authentication and Access Control

Only students with valid UB email addresses may register for Remmogo. Authentication and authorization responsibilities are kept separate from the Rental lifecycle so that access control does not become mixed with rental processing.

| Requirement              | Architectural Response                                                                                         |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| Valid UB students only   | Registration validates the UB email requirement.                                                               |
| Protected system actions | Authentication is required before protected functionality is accessed.                                         |
| Different user roles     | Student accounts support Customer and Owner roles, while Administrator functionality is separately controlled. |
| Rental participation     | Chat and rental information are restricted to authorized participants.                                         |

## 8. Failure and Security Boundaries

| Concern          | Architectural Decision                                                                   | Resulting Consideration                                            |
| ---------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Payments         | Payments remain outside Remmogo. The system records payment confirmations only.          | Remmogo cannot independently verify whether money was transferred. |
| Database access  | Prepared statements are required for database queries.                                   | Reduces the risk of unsafe database queries.                       |
| Rental conflicts | Rental state and item availability are updated together.                                 | Reduces the possibility of conflicting reservations.               |
| Chat access      | Conversations and messages are linked to Rentals and limited to authorized participants. | Requires access-control checks for chat functionality.             |
| Overdue rentals  | The system checks due dates and generates notifications.                                 | Requires scheduled processing and notification handling.           |

The project deliberately keeps payment processing outside the system, while the architecture records Customer and Owner payment confirmations.

## 9. Highest Architectural Risk

The highest architectural risk is maintaining consistent item availability when multiple Customers attempt to rent limited quantities at the same time.

This risk affects the correctness of the Rental lifecycle because an incorrect availability update could allow the same quantity to be reserved more than once.

The architecture addresses this through:

1. A defined `Reserved` rental state.
2. Availability validation during rental creation.
3. Controlled updates to the available quantity.
4. Releasing reserved quantities when a Rental is cancelled.
5. Testing concurrent rental requests.

The Phase 1 design identifies simultaneous limited-quantity requests as a residual risk that should receive attention during implementation and testing.

## 10. Architectural Alternatives Considered

| Alternative                      | Decision     | Reason                                                                                                                                            |
| -------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| In-app payment processing        | Not selected | Adds financial data, external payment-provider integration, security requirements and additional failure cases outside the current project scope. |
| Distributed microservices        | Not selected | Adds operational complexity that is not justified by the expected scale and semester development constraints.                                     |
| No reservation state             | Not selected | Could allow competing rental requests to exceed the available quantity.                                                                           |
| Three-layer modular architecture | Selected     | Provides clear separation between presentation, application logic and data while remaining practical for the semester project.                    |

The alternatives and their consequences were considered in the Phase 1 architecture decisions.

## 11. Architectural Evidence and Testing

The architecture will be evaluated using the quality scenarios and acceptance criteria defined for Remmogo.

| Area                | Evidence to Collect                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------------------------- |
| Login               | Measure authentication time with approximately 50 simultaneous users.                                |
| Search              | Measure search response time using a dataset containing more than 1,000 items.                       |
| Rental transactions | Measure the time required to create and confirm rental records.                                      |
| Availability        | Test simultaneous requests for items with limited quantities.                                        |
| Chat                | Measure the time between sending and receiving messages.                                             |
| Reports             | Measure report generation time using more than 500 rental records.                                   |
| Overdue processing  | Verify that overdue Rentals are identified and notifications are generated within the required time. |

These tests will provide evidence that the architectural design supports the quality requirements rather than relying only on the intended design.

## 12. Summary

The Remmogo architecture is designed around three main architecture drivers: availability integrity, trusted student access, and timely interaction and lifecycle response.

The three-layer architecture separates presentation, application and data responsibilities. Within the application layer, Rental manages the rental lifecycle while ItemListing manages item information and availability, Student / Account handles access, Conversation / Message handles communication, and Administrator / Report handles administrative functions.

The architecture also addresses the main risks identified in Phase 1, particularly conflicting reservations, authentication, external payment confirmation, notifications and rental lifecycle management.
