Primary Actor: Student (Renter)

Preconditions:
Renter has a valid UB account and is signed in.
Item is available in the marketplace.

Main Flow:
Renter searches and selects an item.
Renter clicks “Rent” and specifies rental period.
System records a pending transaction and notifies seller.
Renter confirms external payment by uploading proof of payment.
Seller confirms payment received.
System updates item availability, sets due date, and schedules return and payment reminders.
Both renter and seller receive confirmation notifications.

Postconditions:
Rental record stored with renter, seller, item details, due date, status and periodical payment.
Transaction ready for product return confirmation and later rating of seller.

Alternative Flows:
Item already rented → system blocks duplicate booking and notifies renter.
Payment not confirmed → transaction remains pending for 48 hours until both parties confirm.
Late return → system flags overdue rental and sends notifications.
Cancellation/no‑show → system reverts item to “available” and logs cancellation.
