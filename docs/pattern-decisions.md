
# Design Pattern Decisions
## RREMOGO

### 1. Introduction

The patterns are selected based on the actual requirements of the system, including managing users, item listings, rental transactions, payment confirmations, notifications, and rental status changes.

Each decision explains the problem, the selected pattern, its application, and the reasons for choosing it over a less structured approach.


### 2. Design Pattern Decisions

#### 2.1 Model-View-Controller (MVC) Pattern

**Project-specific problem**

The marketplace has different functionalities, including user registration, item listings, rental requests, payment confirmation, and administration. If the interface, database operations, and business rules are combined in the same code, changes to one feature could affect other parts of the system. This would make the application difficult to maintain and test.

**Selected solution: Model-View-Controller (MVC)**

MVC separates the application into three responsibilities:

- **Model:** Manages application data and business rules, such as users, items, and rental transactions.
- **View:** Displays information to users, including item listings, rental details, and dashboards.
- **Controller:** Receives user requests and coordinates the appropriate application operations.

**Application example**

When a student requests to rent a refrigerator, the controller receives the request, communicates with the rental service and model, and returns the appropriate response to the user interface.

**Why this pattern fits**

MVC is appropriate because the marketplace contains several interconnected features that need to remain organised. Separating the interface from business logic makes it easier to modify and test individual parts of the application.

**Trade-offs**

MVC introduces additional files and responsibilities that developers must manage. However, this complexity is justified by the benefits of maintaining a structured application.

---

#### 2.2 Repository Pattern

**Project-specific problem**

The marketplace frequently accesses and updates related database information. For example, processing a rental request requires retrieving the student, item, seller, availability, and rental information. Writing database queries directly inside every feature could lead to duplicated queries and inconsistent database operations.

**Selected solution: Repository Pattern**

The Repository Pattern provides a separate layer responsible for retrieving, creating, updating, and deleting database records.

**Application example**

A RentalRepository can handle operations such as:
- Retrieving a student's rental history.
- Checking existing rental records.
- Creating a new rental transaction.
- Updating rental status.
- Retrieving overdue rentals.

**Why this pattern fits**

The marketplace depends heavily on database operations. Centralising data access reduces duplication and allows business logic to interact with the database through a consistent interface.

It also makes testing easier because database access can be replaced with test implementations where appropriate.

**Trade-offs**

The additional repository layer introduces more code and abstraction. For a small application, some operations could be handled directly by the framework's existing data-access facilities. The pattern should therefore be applied where it provides genuine value.

---

#### 2.3 Observer Pattern

**Project-specific problem**

Several activities in the marketplace require other users to be informed when something happens. For example, when a student submits a rental request, the seller needs to be notified. When a seller confirms payment, the student should receive an update.

If the rental management code directly handles every notification, the system becomes tightly coupled. Adding another notification method could require changes to several rental operations.

**Selected solution: Observer Pattern**

The Observer Pattern allows interested components to receive notifications when a particular event occurs.

**Application example**

When a rental request is submitted:
1. The rental transaction is recorded.
2. A rental-request event is generated.
3. The notification component receives the event.
4. The seller receives a notification.

The same approach can be used for payment confirmation, overdue rentals, and return confirmation.

**Why this pattern fits**

The marketplace has multiple events that require notifications. The Observer Pattern separates the event-producing functionality from the notification functionality, making it easier to extend the system.

**Trade-offs**

Event handling introduces additional complexity. The system must manage failed notifications, duplicate events, and notification delivery.

---

#### 2.4 State Pattern

**Project-specific problem**

A rental transaction moves through several stages. A student first selects an item, submits a request, uploads proof of payment, and waits for the seller to confirm payment. After the item is handed over, the rental becomes active until it is returned.

Without clear state management, the system could allow invalid operations, such as completing a rental before payment confirmation or making an item available while it is still being rented.

**Selected solution: State Pattern**

The State Pattern manages an object's behaviour according to its current state.

**Application example**

The rental transaction can move through the following states:

- Browsing
- ItemSelected
- Reserved
- PendingTransaction
- WaitingForPOP
- AwaitingOwnerConfirmation
- ActiveRental
- Overdue
- Completed
- Cancelled

For example, a rental can only move to ActiveRental after the required payment confirmation and handover conditions have been satisfied.

A cancelled rental cannot proceed directly to Completed.

**Why this pattern fits**

Rental transactions have a clearly defined lifecycle, and each stage has different permitted operations. The State Pattern provides a structured way to control these transitions and prevent invalid actions.

**Trade-offs**

Creating separate state classes may introduce unnecessary complexity if the application only requires simple status checks. The pattern should be used when state-specific behaviour becomes sufficiently complex. Database constraints and transaction validation are still necessary to protect data integrity.

---

#### 2.5 Strategy Pattern

**Project-specific problem**

The marketplace needs to communicate with users about rental requests, payment confirmations, upcoming due dates, and overdue items. Different notification channels may be required, including email and in-app notifications.

Implementing all notification methods in one large component could result in complicated conditional statements and make future changes difficult.

**Selected solution: Strategy Pattern**

The Strategy Pattern allows different notification delivery algorithms to be implemented independently and selected when required.

**Application example**

The system could support:
- Email notification strategy.
- In-app notification strategy.
- Push notification strategy, if introduced.

The notification service selects the appropriate strategy depending on the notification requirements and available services.

**Why this pattern fits**

The marketplace may introduce additional notification channels in the future. The Strategy Pattern allows new delivery methods to be added without rewriting the rental management functionality.

**Trade-offs**

Each strategy requires a separate implementation. This may be excessive if the system only supports one notification channel.

---

#### 2.6 Factory Pattern

**Project-specific problem**

If the marketplace supports different notification types, the system needs to create the appropriate notification objects. Creating these objects directly throughout the application could duplicate object-creation logic and make future extensions more difficult.

**Selected solution: Factory Pattern**

The Factory Pattern centralises the creation of related objects and hides the creation details from the components that use them.

**Application example**

A NotificationFactory could create the appropriate notification implementation based on the selected type, such as email or in-app notification.

**Why this pattern fits**

The pattern is useful when the marketplace supports multiple notification implementations. It separates object creation from notification processing and makes it easier to introduce new notification types.

**Trade-offs**

The factory introduces another abstraction. If notification creation remains simple, using the factory may not provide enough benefit to justify the additional code.

---

### 3. Summary of Design Decisions

| Pattern | Specific project problem addressed | Main reason for selection |
|---|---|---|
| MVC | Interface, business logic, and data responsibilities becoming mixed. | Separates application responsibilities. |
| Repository | Repeated and inconsistent database operations. | Centralises data access. |
| Observer | Rental events need to notify different users. | Separates event handling from notification delivery. |
| State | Rental transactions must follow valid stages. | Controls state-dependent behaviour. |
| Strategy | Different notification delivery methods are required. | Allows notification methods to be changed independently. |
| Factory | Notification objects need to be created consistently. | Centralises object creation. |

### 4. Pattern Interaction

The selected patterns can work together during a rental transaction.

For example, when a student requests to rent an item:

1. The MVC controller receives the rental request.
2. The rental service applies the relevant business rules.
3. The Repository retrieves the item and creates the rental record.
4. The State Pattern governs the rental's permitted status transition.
5. The Observer Pattern publishes a rental-request event.
6. The notification service uses the Strategy Pattern to select a delivery method.
7. The Factory Pattern can create the required notification object.

This interaction supports separation of responsibilities and reduces unnecessary dependencies between system components.

### 5. Conclusion

The design patterns selected for the Rremmogo address specific problems associated with application organisation, database access, rental lifecycle management, and notifications.

MVC and Repository support application structure and data access, while Observer, State, Strategy, and Factory address event handling, transaction states, notification flexibility, and object creation.

The patterns will only be implemented where their benefits justify their complexity. Their actual use will be verified against the application source code as development progresses.
