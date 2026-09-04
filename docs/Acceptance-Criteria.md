Successful Rental Request

Scenario: Customer successfully requests to rent an available item

Given the customer has a valid UB account, is signed in, and the selected item is available in the marketplace
When the customer selects the item, clicks “Rent”, specifies the rental period, and submits the rental request
Then the system shall create a pending rental transaction, record the customer, owner, item details, rental period, and notify the owner of the rental request.

 Payment Confirmation and Rental Activation

Scenario: Customer and owner confirm payment for a rental

Given a pending rental transaction exists and the customer has uploaded proof of payment
When the owner confirms that payment has been received
Then the system shall update the transaction status to confirmed, record the payment confirmation, set the rental due date, update item availability, and send confirmation notifications to both parties

Prevent Duplicate Booking of an Unavailable Item

Scenario: Customer attempts to rent an already rented or reserved item

Given the customer is signed in and browsing the catalog, the item should not appear in the availability list.

Handling Payment Not Confirmed

Scenario: Rental remains pending when payment is not confirmed

Given a rental request has been created but payment confirmation has not been received from both parties
When 48 hours pass without payment confirmation
Then the system shall keep the transaction pending, notify the customer and owner, and prevent the rental from becoming active.

Return and Overdue Management

Scenario: System handles a late return

Given an active rental has reached its due date and the item has not been confirmed as returned
When the return deadline passes
Then the system shall mark the rental as overdue, notify both the customer and owner, and record the overdue status in the rental history.

