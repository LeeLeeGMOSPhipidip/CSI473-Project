We chose to use a three-tier architecture consisting of:
1. Presentation Layer
Registration and login screens
Item browsing and searching
Rental request screens
Chat interface
Reporting and administration screens

2. Application Layer
Authentication and authorization
Rental management
Item availability management
Payment and return confirmations
Notifications
Ratings and reviews
Report generation

3. Data Layer
Student accounts
Item listings
Rental records
Messages
Ratings
Reports
Confirmation records

Positive Consequences
Clear separation of responsibilities between system layers.
Easier debugging and maintenance.
Business rules can be updated without affecting the user interface.
Database changes can be managed independently from the presentation layer.
Suitable for a student project with limited time and resources.
Supports future expansion if additional features are required.

Negative Consequences
All layers depend on the central application layer.
The application must be deployed as a single system.
May not scale as efficiently as a microservices architecture.
Additional layers can introduce slightly more communication overhead between components.

Alternative Architecture: Distributed Microservices Architecture

Rejected because:
Requires separate deployment of services.
Increases development and testing complexity.
Introduces communication and coordination challenges between services.

Risks
The main architectural risk is maintaining correct item availability when multiple Customers request the same item at the same time. If reservation logic is not handled correctly, duplicate bookings may occur.

To reduce this risk:
Item quantities will be validated before reservation.
Reservations will be created immediately after acceptance.
Cancelled rentals will automatically release reserved quantities.
