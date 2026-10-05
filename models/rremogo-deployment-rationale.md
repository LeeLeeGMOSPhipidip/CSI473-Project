# Rremogo Lab 8 — Deployment Diagram Rationale

## Purpose and traceability
This deployment model supports the core Rremogo rental workflow: a Student uses the Rremogo interface to request an available item for rental. It is linked to the existing **Rent an Item** scenario and the rule that a valid UB account must be signed in.

## Nodes and responsibilities
- **Student (Customer / Owner):** uses Rremogo as the system client.
- **Web Browser:** hosts the Rremogo user interface.
- **Rremogo Web Application:** presentation/UI layer.
- **Rremogo Backend / API:** implements rental-request processing and business rules such as availability and reservation.
- **Rremogo Database:** persists users, item listings, rentals and payment-proof/confirmation information.
- **UB Authentication Service:** external dependency used to validate/sign in a UB account.
- **External payment:** payment occurs directly between Customer and Owner and is deliberately not represented as a Rremogo payment gateway.

## Trust/network boundaries
The Rremogo application components are shown inside the Rremogo boundary. UB authentication is outside the boundary because it is an external dependency. The browser is also outside the application boundary because it is the user's client.

## Design rationale
A separate backend/API is used instead of placing rental rules in the browser because business rules such as availability and reservation must be enforced centrally. Persistent rental state is stored in a database so that item availability and rental status are not dependent on a user's browser session.

## Alternative considered
A single application node could combine the web interface and backend. The selected separation is preferred because it keeps presentation separate from rental/business logic and gives a clearer boundary for the core service.

## Consequence accepted
The separated design introduces an additional application boundary and API communication, but this is accepted because it makes the rental rules easier to enforce consistently and keeps the deployment structure clear.

## Integrity concern
The main integrity concern is inconsistent rental state if two students request the same item at nearly the same time. The backend and database must perform the availability check and reservation update as one protected operation/transaction so that an item cannot be reserved twice.
