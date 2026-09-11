# Remmogo Functional Requirements

## Account access

**FR-01 - Register account**  
The system shall allow a Student to register an Account using a valid University of Botswana email address.

**FR-02 - Sign in and sign out**  
The system shall allow a registered Student or Administrator to sign in and sign out securely.

## Item listings and discovery

**FR-03 - Create item listing**  
The system shall allow an Owner to create and publish an ItemListing for rent.

**FR-04 - Record listing details**  
The system shall require each ItemListing to contain a name, Category, condition, description, rental price, total quantity and available quantity.

**FR-05 - Categorise listing**  
The system shall require each ItemListing to be assigned to exactly one Category.

**FR-06 - Search listings**  
The system shall allow a Customer to search for ItemListings by item name or Category.

**FR-07 - Display available listings**  
The storefront shall display only ItemListings with an unreserved available quantity greater than zero.

**FR-08 - View listing details**  
The system shall allow a Customer to view the details, rental price and available quantity of an ItemListing.

**FR-09 - Manage own listing**  
The system shall allow an Owner to edit an owned ItemListing, update its availability or remove it from the storefront.

## Rental workflow

**FR-10 - Submit rental request**  
The system shall allow a signed-in Customer to submit a Rental request for a specified ItemListing, quantity and rental period.

**FR-11 - Validate and reserve quantity**  
Before creating a Rental, the system shall verify that the requested quantity is available. When a valid request is created, the system shall reserve that quantity so that it cannot be allocated to another Rental.

**FR-12 - Review rental request**  
The system shall allow the Owner of the ItemListing to view, accept or reject a Rental request.

**FR-13 - Release reserved quantity**  
If a Rental request is rejected, cancelled or expires, the system shall release its reserved quantity and update the ItemListing's availability.

**FR-14 - Rental communication**  
The system shall provide a Conversation through which the Customer and Owner participating in a Rental can exchange Messages.

**FR-15 - Confirm external payment**  
The system shall allow the Customer to confirm that payment was made outside Remmogo and the Owner to confirm that payment was received. The system shall not process, transfer or hold payment funds.

**FR-16 - Confirm item handover**  
After both payment confirmations have been recorded, the system shall allow the Customer and Owner to confirm item handover before the Rental becomes Active.

**FR-17 - Confirm item return**  
The system shall allow both the Customer and Owner to confirm the return of the item and record its condition on return.

**FR-18 - Complete rental and restore availability**  
After both return confirmations have been recorded, the system shall mark the Rental as Completed and restore the returned quantity to the ItemListing.

**FR-19 - Record overdue rental**  
If the expected return date passes before the required return confirmations are recorded, the system shall mark the Rental as Overdue.

**FR-20 - Notify participants**  
The system shall notify the relevant Customer and Owner when a Rental request is created, payment remains unconfirmed, a Rental is activated, a return is due or overdue, or a Rental is cancelled.

## Ratings, reporting and administration

**FR-21 - Rate participant**  
The system shall allow the Customer and Owner to rate each other only after both return confirmations have been recorded and the Rental is Completed.

**FR-22 - Submit report**  
The system shall allow a Student to report an Account, ItemListing or Rental-related issue for administrative review.

**FR-23 - Manage accounts**  
The system shall allow an Administrator to review Account issues and suspend or deactivate an Account when authorised.

**FR-24 - Manage listings**  
The system shall allow an Administrator to review and remove an ItemListing when it violates applicable rules.

**FR-25 - Manage disputes**  
The system shall allow an Administrator to review and record the resolution of reported Rental disputes.

**FR-26 - Generate reports**  
The system shall allow an Administrator to generate reports about user activity, ItemListings, Rentals and disputes.
