# Remmogo Interfaces and Ports

## 1 Purpose and traceability

This section defines the stable interfaces around Remmogo's application core. The boundaries keep rental rules independent of the web framework, database implementation, notification mechanism and external PDF-rendering application.

The design supports:

- FR-06, FR-07 and FR-11: search and correct item availability;
- FR-08 and FR-09: payment and return confirmation;
- FR-13, BR-08 and QS-05: administrative PDF report generation;
- BR-04: an item may be rented only when sufficient quantity is available;
- QS-03: a valid rental request is stored within two seconds;
- QS-06: overdue rentals are updated and participants are notified;
- QS-07: recovery must not create duplicate or corrupt transactions.

## 2 Dependency direction

```text
Web controllers / scheduled jobs
              |
              v
       Inbound ports
  RentalUseCase, ReportExportUseCase
              |
              v
    Application services and domain rules
              |
              v
       Outbound ports
  RentalRepository, ItemAvailabilityPort,
  NotificationPort, PdfReportPort
              ^
              |
 Database, email and PDF-service adapters
```

Dependencies point towards the application core. Controllers call inbound ports. Application services depend on outbound-port interfaces. Concrete database, notification and PDF clients implement those outbound interfaces.

Framework annotations such as Spring `@RestController`, JPA `@Entity` and HTTP-client configuration belong in adapters, not in the port interfaces.

## 3 Port catalogue

| Port | Direction | Called by | Implemented by | Responsibility |
|---|---|---|---|---|
| `RentalUseCase` | Inbound / provided | Student and Owner controllers | `RentalApplicationService` | Request a rental, accept/reject it and record payment or return confirmations. |
| `ReportExportUseCase` | Inbound / provided | Administrator report controller | `ReportExportService` | Start or retrieve an idempotent PDF report export. |
| `RentalRepository` | Outbound / required | Rental application service | Database repository adapter | Load and save rental aggregates while protecting concurrent updates. |
| `ItemAvailabilityPort` | Outbound / required | Rental application service | Listing/JPA adapter | Atomically reserve or release item quantity. |
| `NotificationPort` | Outbound / required | Rental and overdue services | Email/in-app notification adapter | Send secondary notifications after the core state change succeeds. |
| `PdfReportPort` | Outbound / required | Report export service | HTTP adapter for the PDF application | Render structured report content as a PDF without exposing the Remmogo database. |

## 4 Inbound ports

### 4.1 RentalUseCase

`RentalUseCase` is the application boundary for the core rental workflow. It accepts commands containing identifiers and user-entered values, applies authorisation and business rules, coordinates availability and persistence, and returns a stable result.

```java
public interface RentalUseCase {
    RentalResult requestRental(RequestRentalCommand command);
    RentalResult decideRental(DecideRentalCommand command);
    RentalResult confirmPayment(ConfirmRentalEventCommand command);
    RentalResult confirmReturn(ConfirmReturnCommand command);
}
```

The controller is responsible for HTTP parsing and basic format validation. The application service remains responsible for rules that require current domain data, including:

- the Customer and Owner must be different Students;
- the listing must be active and have enough available quantity;
- only the listing Owner may accept or reject the request;
- only rental participants may confirm payment or return;
- each party may confirm each event once;
- invalid lifecycle transitions are rejected.

### 4.2 ReportExportUseCase

```java
public interface ReportExportUseCase {
    ReportExportResult generateReport(GenerateReportCommand command);
    Optional<ReportExportResult> findExport(UUID exportId, UUID administratorId);
}
```

The inbound port requires an idempotency key. Repeating the same request with the same key returns the existing export; reusing the key with different filters is rejected.

## 5 Outbound ports

### 5.1 RentalRepository

```java
public interface RentalRepository {
    Optional<RentalSnapshot> findById(UUID rentalId);
    Optional<RentalSnapshot> findByIdempotencyKey(String key);
    RentalSnapshot save(RentalSnapshot rental);
}
```

The interface uses application/domain models rather than JPA entities. The adapter translates between `RentalSnapshot` and the persistence representation.

### 5.2 ItemAvailabilityPort

```java
public interface ItemAvailabilityPort {
    AvailabilityResult reserve(UUID listingId, int quantity, long expectedVersion);
    AvailabilityResult release(UUID listingId, int quantity, long expectedVersion);
}
```

This port represents an atomic business operation, not generic CRUD. `reserve` succeeds only when the listing is active and sufficient quantity remains. The expected version allows optimistic concurrency control. Two Customers requesting the final unit must produce exactly one success.

### 5.3 NotificationPort

```java
public interface NotificationPort {
    void rentalRequested(RentalNotification notification);
    void rentalStatusChanged(RentalNotification notification);
    void rentalOverdue(RentalNotification notification);
}
```

Notifications are secondary effects. The rental transaction is committed first, and notification failure must not roll back or duplicate the rental. Failed deliveries are logged for retry using the rental ID and event type as an idempotency reference.

### 5.4 PdfReportPort

```java
public interface PdfReportPort {
    PdfRenderResult render(PdfRenderRequest request);
    Optional<PdfRenderResult> findByCorrelationId(String correlationId);
}
```

The PDF adapter converts the application request into the external application's HTTP/JSON format. The core service never depends on the external client's SDK or response classes.

The adapter must:

- use TLS and service credentials stored outside source code;
- apply connection and response timeouts;
- translate external failures into stable application outcomes;
- propagate the correlation identifier;
- avoid logging report rows containing personal data;
- reconcile an unknown timeout outcome before starting another render.

## 6 Application-service coordination

### 6.1 Request rental

1. `RentalController` maps the authenticated Student and JSON body to `RequestRentalCommand`.
2. `RentalApplicationService`, implementing `RentalUseCase`, checks for an existing idempotent result.
3. It loads the listing and verifies the Student is not its Owner.
4. It calls `ItemAvailabilityPort.reserve(...)`.
5. If reservation succeeds, it creates and stores the `REQUESTED` rental through `RentalRepository` in the same transaction boundary.
6. After commit, it calls `NotificationPort.rentalRequested(...)`.
7. The controller maps the result to the API response.

If reservation fails, no rental is stored and the application returns `ITEM_UNAVAILABLE`. If the persistence transaction fails after reservation, the transaction must roll back both changes or execute a compensating release before returning failure.

### 6.2 Generate PDF report

1. `AdminReportController` maps filters and the `Idempotency-Key` header to `GenerateReportCommand`.
2. `ReportExportService`, implementing `ReportExportUseCase`, authorises the Administrator and checks for an existing export.
3. It queries a consistent report-data snapshot and stores the export as `PROCESSING`.
4. It calls `PdfReportPort.render(...)` with prepared columns and rows.
5. On success, it stores the file reference, checksum and `COMPLETED` status.
6. On a known failure, it stores `FAILED` with a stable code.
7. On timeout, it preserves the correlation ID and checks `findByCorrelationId(...)` before allowing a new render.

## 7 Stable outcomes and exceptions

Port interfaces return explicit results for expected business outcomes. Exceptions are reserved for unexpected technical failures.

| Outcome | Meaning | API mapping |
|---|---|---|
| `SUCCESS` | Operation completed. | 200 or 201 |
| `ITEM_UNAVAILABLE` | Listing has insufficient quantity or is inactive. | 409 |
| `INVALID_STATE` | Requested lifecycle transition is not permitted. | 409 |
| `FORBIDDEN` | Authenticated user is not allowed to perform the action. | 403 |
| `NOT_FOUND` | Requested rental, listing or export does not exist. | 404 |
| `DUPLICATE_CONFIRMATION` | The same party already confirmed the event. | 409; return current state |
| `PDF_UNAVAILABLE` | PDF dependency is unavailable. | 503 |
| `PDF_TIMEOUT` | PDF outcome is unknown and requires reconciliation. | 504 or pending response |

## 8 Dependency injection

Application services receive required ports through constructor injection:

```java
public final class RentalApplicationService implements RentalUseCase {
    private final RentalRepository rentals;
    private final ItemAvailabilityPort availability;
    private final NotificationPort notifications;

    public RentalApplicationService(
            RentalRepository rentals,
            ItemAvailabilityPort availability,
            NotificationPort notifications) {
        this.rentals = rentals;
        this.availability = availability;
        this.notifications = notifications;
    }
}
```

Constructor injection makes dependencies explicit, supports unit tests with in-memory fakes and prevents the core service from locating dependencies through global state or a service locator.

## 9 Testing the boundaries

| Test | Ports replaced | Expected evidence |
|---|---|---|
| Two concurrent requests for the last unit | Fake/transactional `ItemAvailabilityPort` | One succeeds, one returns `ITEM_UNAVAILABLE`, quantity never becomes negative. |
| Duplicate rental submission | In-memory `RentalRepository` | Same idempotency key returns the same rental ID. |
| Duplicate payment confirmation | In-memory repository | One confirmation per party; repeat returns current state without adding another row. |
| Notification outage | Failing `NotificationPort` | Rental remains stored once and notification is marked for retry. |
| PDF timeout after external acceptance | Fake `PdfReportPort` | Reconciliation returns the original document; no duplicate export is produced. |
| PDF service unavailable | Failing `PdfReportPort` | Export receives stable failure code; rental data remains unchanged. |

## 10 Cost and benefit

The greatest benefit comes from `ItemAvailabilityPort`: it makes the double-booking rule explicit and independently testable while allowing the database adapter to implement locking or optimistic concurrency. The cost is additional interfaces, mapping code and more classes.

The design will be confirmed by a concurrency integration test in which two requests compete for the final available unit. The architecture is accepted only if exactly one reservation succeeds and the stored quantity never becomes negative.

Not every class receives an interface. Simple value objects, domain records and framework-independent utilities remain concrete because an interface would add indirection without providing a meaningful boundary.

