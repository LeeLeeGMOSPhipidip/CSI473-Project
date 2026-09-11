# Remmogo Architecture Drivers

## Purpose

This note records the Phase 1 quality and constraint drivers that will guide architecture work in Phase 2. The drivers come from the approved Remmogo scope, functional requirements, acceptance criteria and measurable quality scenarios rather than from a preferred framework.

## AD-01 Availability integrity

Remmogo must prevent conflicting Rentals when several Customers request the same limited quantity.

- **Source:** FR-07, FR-11, Prevent Duplicate Booking acceptance criterion, availability business rule.
- **Required response:** Validate availability and reserve quantity as one controlled operation. Exclude fully reserved quantity from storefront results. Release reserved quantity after cancellation or completion.
- **Consequence:** Reservation and timeout logic add lifecycle complexity, but prevent available quantity from becoming negative or being promised to more than one Customer.

## AD-02 Trusted student access

Registration and protected actions must be restricted to valid University of Botswana accounts.

- **Source:** FR-01, FR-02, approved scope and Quality Scenario 1.
- **Required response:** Separate authentication and authorisation from Rental logic. Support 50 simultaneous login attempts with responses within five seconds.
- **Consequence:** Restricting access improves accountability but excludes users outside the UB community and creates a dependency on reliable email verification.

## AD-03 Timely interaction and lifecycle response

Search, chat, Rental creation and overdue processing must meet measurable response targets.

- **Source:** Quality Scenarios 2, 3, 4 and 6.
- **Required response:** Keep presentation, Rental rules, persistence and notifications separated so that performance and failures can be tested independently.
- **Consequence:** Clear component boundaries improve testability and responsiveness but require explicit interfaces and error handling.

## Alternatives considered

### Alternative A In-app payment processing

Remmogo could integrate with a payment provider and process funds directly.

- **Decision:** Rejected for the initial implementation.
- **Reason:** It introduces financial-data handling, provider integration, additional failure modes and security obligations beyond the approved scope.
- **Accepted consequence:** Students pay externally, so Remmogo records the Customer and Owner confirmations but cannot independently verify or reverse payment.

### Alternative B Distributed microservices

Authentication, listings, Rentals, chat and reporting could be deployed as separate services.

- **Decision:** Rejected for the semester implementation.
- **Reason:** Independent deployment and distributed coordination add operational complexity without evidence that the expected scale requires them.
- **Accepted consequence:** A layered modular application is easier to implement and test, but module boundaries must remain clear to avoid tight coupling.

### Alternative C No reservation state

An ItemListing could remain available until payment and handover are confirmed.

- **Decision:** Rejected.
- **Reason:** Competing requests could exceed the available quantity and create conflicting Rentals.
- **Accepted consequence:** A visible Reserved state and a confirmation timeout add lifecycle rules but protect availability integrity.
