The system stores information about students, listings, rentals, messages, notifications and reports in a database. If business services communicate directly with database tables, the code becomes difficult to maintain and tedious to test.

We have decided to use the Repository Pattern to manage data access. Repository classes will handle all database operations while business services focus on business rules. Business services such as RentalService interact with repository interfaces rather than database queries directly. This improves maintainability, testability and flexibility because database-related changes can be made inside repository classes without affecting business logic.

Examples include:
StudentRepository
ListingRepository
RentalRepository

New flow:
RentalService (interface to interact with functions and stored data)
      v
RentalRepository (contains queries for the database)
      v
Database 


Positive Consequences
Cleaner architecture.
Better maintainability.
Easier testing.
Supports future database changes.

Negative Consequences
More classes must be created.
Slight increase in implementation effort.

Alternatives Considered
Direct Database Access
Business services execute SQL queries directly.

Rejected because:
Creates tight coupling between business logic and the database.
Difficult to test.
Harder to maintain.
