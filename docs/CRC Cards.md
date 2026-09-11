# Remmogo CRC Cards

## CRC-01 Student and Account

**KNOW**

- UB email
- Password
- Student name
- Account status
- Role in a rental transaction

**DO**

- Register an account
- Sign in and sign out
- Manage account details

**PROTECT**

- Require a valid UB email address
- Prevent duplicate accounts

**COLLABORATE**

- Administrator for account issues
- Rental when participating as a Customer or Owner

**Rationale:** Student and Account hold the identity and access information needed to ensure that only valid UB users participate in Remmogo.

## CRC-02 Owner

**KNOW**

- Owned ItemListings
- Rental requests for owned ItemListings

**DO**

- Upload and list items
- Confirm payment received
- Confirm item handover and return
- Rate Customers

**PROTECT**

- Ensure listing details are complete
- Prevent unavailable items from being offered for rent

**COLLABORATE**

- ItemListing
- Rental
- Customer
- Conversation and Message

**Rationale:** Owner represents the responsibilities a Student has when providing an item for rent.

## CRC-03 Customer

**KNOW**

- Rental history
- Requested ItemListings

**DO**

- Search for items
- Initiate rental transactions
- Confirm payment made
- Confirm item handover and return
- Rate Owners

**PROTECT**

- Ensure rental requests are made only for available items

**COLLABORATE**

- ItemListing
- Rental
- Owner
- Conversation and Message

**Rationale:** Customer represents the responsibilities a Student has when requesting to rent an item.

## CRC-04 ItemListing

**KNOW**

- Listing ID
- Item name
- Condition
- Description
- Rental price
- Available quantity
- Category
- Listing status

**DO**

- Provide item details
- Update availability

**PROTECT**

- Prevent available quantity from dropping below zero
- Require a Category
- Exclude reserved or unavailable items from availability results

**COLLABORATE**

- Owner
- Category
- Rental

**Rationale:** ItemListing owns the information and availability state of an item offered for rent.

## CRC-05 Category

**KNOW**

- Category ID
- Category name

**DO**

- Classify ItemListings
- Support searching and filtering by Category

**PROTECT**

- Ensure each ItemListing belongs to one Category
- Prevent duplicate Category names

**COLLABORATE**

- ItemListing

**Rationale:** Category owns classification information used to organise and search ItemListings consistently.

## CRC-06 Rental

**KNOW**

- Rental ID
- Start and return dates
- Rental status
- Customer
- Owner
- ItemListing
- Rental quantity

**DO**

- Create the rental record
- Enforce the return date
- Flag an overdue rental
- Close the Rental after confirmed return
- Coordinate rental notifications

**PROTECT**

- Prevent duplicate rentals for unavailable items
- Require payment confirmation before activation
- Require return confirmation before rating

**COLLABORATE**

- Customer
- Owner
- ItemListing
- PaymentConfirmation
- ReturnConfirmation
- Rating
- Conversation and Message
- Administrator

**Rationale:** Rental owns the lifecycle information and coordinates the rules, state changes and exceptions of a rental transaction.

## CRC-07 Rating

**KNOW**

- Rating ID
- Score
- Comment
- Associated Rental

**DO**

- Store feedback
- Link feedback to a completed Rental

**PROTECT**

- Allow ratings only after both parties confirm the item’s return

**COLLABORATE**

- Rental
- Customer and Owner as rating participants

**Rationale:** Rating owns evaluation information and ensures that feedback is submitted only at the correct point in the rental lifecycle.

## CRC-08 Conversation and Message

**KNOW**

- Conversation ID
- Message ID
- Sender
- Time sent
- Message content

**DO**

- Store messages
- Deliver communication between rental participants

**PROTECT**

- Link messages to the appropriate Rental
- Restrict communication to the relevant participants

**COLLABORATE**

- Customer
- Owner
- Rental

**Rationale:** Conversation and Message hold communication records and keep rental-related communication traceable.

## CRC-09 Administrator and Report

**KNOW**

- Administrator ID
- Administrative privileges
- Account policies
- Dispute information
- Report details

**DO**

- Manage account issues
- Resolve rental disputes
- Generate reports

**PROTECT**

- Restrict administrative actions to authorised Administrators
- Apply applicable university and residence rules

**COLLABORATE**

- Student and Account
- Rental
- ItemListing
- Report

**Rationale:** Administrator and Report support oversight, account management, dispute handling and reporting.
