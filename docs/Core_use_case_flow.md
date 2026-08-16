Primary Actor: Student (Customer)

Use Case: "Request to rent item"

Preconditions:
Customer has a valid UB account and is signed in.
Item is available in the marketplace.

Main Flow:
Customer searches and selects an item.
Customer clicks “Rent” and specifies rental period.
System records a pending transaction and notifies seller.
Customer confirms external payment by uploading proof of payment.
Owner confirms payment received.
Customer confirms item handover.
System updates item availability, sets due date, and schedules return and future payment reminders.
Both Customer and Owner receive confirmation notifications.

Postconditions:
Rental record stored with Customer, Owner, item details, due date, status and periodical payment.
Transaction ready for product return confirmation and later rating of seller.

Alternative Flows:
Item already rented → system blocks duplicate booking and notifies Customer.
Payment not confirmed → transaction remains pending for 48 hours until both parties confirm.
Late return → system flags overdue rental and sends notifications.
Cancellation/no‑show → system reverts item to “available” and logs cancellation.
