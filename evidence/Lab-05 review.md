High Risk Inconsistency
One of our requirement states “The storefront shall display only items that are currently available for rent.” However, our lifecycle and State machine Diagrams allowed items in a Pending state (when payment is not confirmed yet) but will still appear as available in the system. This creates a risk of double-booking and can be misleading.

Revisions:
•	Change requirement wording to “The storefront shall display only items that are currently available for rent excluding reserved items”
•	Update sequence and activity diagrams to show item status changing to “reserved” once a request is made.
•	Update RentalTransaction state machine to include a Reserved state between Pending and Rented.
•	PROTECT against double booking during Pending/Reserved state in CRC card for Item class.

Plan for Final review:
•	Verify requirements and models all use the same term “Reserved.” And that the traceability matrix links the revised requirement.
•	Add an acceptance criteria – When an item is pending/reserved and another customer searches for it, then it should not appear in the availability listings.
•	Ensure all models are consistent with each other.



