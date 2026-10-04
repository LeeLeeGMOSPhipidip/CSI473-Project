Failure-recovery
Duplicate booking Scenario: Two customers Request the same item

Normal Scenario
1.	Customer submits rental request
2.	System checks quantity
3.	Quantity = 1
4.	System reserves item
5.	Rental request accepted

Failure Scenario
1.	Customer A requests laptop
2.	Customer B requests same laptop
3.	Both requests arrive simultaneously

Recovery Mechanism
1.	Database transaction begins
2.	System locks listing record
3.	Availability checked
4.	First request reserves item
5.	Quantity updated
6.	System unlocks listing record
7.	Second request rechecks quantity
8.	System returns
9.	ITEM_NOT_AVAILABLE

Outcome:
1.	Only one request succeeds.
2.	Data remains consistent.
