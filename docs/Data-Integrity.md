Data Integrity Rules
DI-01 Unique Student Account - Every UB email address must be registered only once.
DI-02 Valid Listings - Every ItemListing must be linked to an existing StudentID.
DI-03 Rental Integrity - An ActiveRental must be from an approved RentalRequest.
DI-04 Availability Protection - The total active rentals for an item cannot exceed available quantity.
DI-05 Message Integrity - Messages must reference valid sender and receiver accounts.

Main Integrity Risk
A duplicate booking may occur if more than one customer request the same item at the same time.

Protection
•	Database transaction locking
•	Availability validation

Verification Test
1.	Create a listing with quantity = 1.
2.	Send two rental requests simultaneously.
3.	Verify that only one request succeeds.
4.	Verify quantity becomes 0.
5.	Verify second request receives an availability error.
