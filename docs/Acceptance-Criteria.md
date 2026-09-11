# Remmogo Acceptance Criteria

## AC-01 Register with a valid UB email

**Requirements:** FR-01

**Given** a Student does not already have an Account  
**When** the Student submits valid registration details containing a valid University of Botswana email address  
**Then** the system shall create the Account and prevent another Account from being registered with the same email address.

## AC-02 Reject an invalid registration email

**Requirements:** FR-01

**Given** a person is completing the registration form  
**When** the person submits an email address that is not a valid University of Botswana email address  
**Then** the system shall reject the registration, display a validation message and not create an Account.

## AC-03 Create a complete item listing

**Requirements:** FR-03, FR-04, FR-05

**Given** an Owner is signed in  
**When** the Owner submits a name, Category, condition, description, rental price, total quantity and available quantity  
**Then** the system shall create the ItemListing and make it available in the storefront when its unreserved available quantity is greater than zero.

## AC-04 Search and display available listings

**Requirements:** FR-06, FR-07, FR-08

**Given** a Customer is signed in and ItemListings exist  
**When** the Customer searches by item name or Category  
**Then** the system shall display matching ItemListings with unreserved available quantity greater than zero and allow the Customer to view their details.

## AC-05 Hide rented or fully reserved listings

**Requirements:** FR-07, FR-11

**Given** an ItemListing's full available quantity is rented or reserved  
**When** a Customer searches or browses the storefront  
**Then** the system shall not display that ItemListing as available.

## AC-06 Submit a successful rental request

**Requirements:** FR-10, FR-11, FR-20

**Given** a Customer has a valid Account, is signed in, and the requested ItemListing quantity is available  
**When** the Customer specifies a valid rental period and quantity and submits the Rental request  
**Then** the system shall create the Rental in the Requested state, record the Customer, Owner, ItemListing, quantity and rental period, reserve the requested quantity and notify the Owner.

## AC-07 Prevent duplicate booking

**Requirements:** FR-07, FR-10, FR-11

**Given** an ItemListing has no unreserved quantity available for the requested rental period  
**When** a Customer attempts to submit a Rental request  
**Then** the system shall reject the request, display an unavailable-item message, create no Rental and leave existing Rentals unchanged.

## AC-08 Owner accepts a rental request

**Requirements:** FR-12

**Given** a Rental is in the Requested state and the signed-in Student owns its ItemListing  
**When** the Owner accepts the request  
**Then** the system shall change the Rental to the Reserved state and retain the quantity reservation.

## AC-09 Rejected or cancelled request releases quantity

**Requirements:** FR-12, FR-13, FR-20

**Given** a Rental has reserved an ItemListing quantity  
**When** the Owner rejects the request or either party validly cancels it before activation  
**Then** the system shall change the Rental to the Cancelled state, release the reserved quantity, update availability and notify both parties.

## AC-10 Two-party payment confirmation

**Requirements:** FR-15

**Given** a Rental is Reserved and payment occurred outside Remmogo  
**When** the Customer confirms making payment and the Owner confirms receiving payment  
**Then** the system shall create one PaymentConfirmation for each party and mark the payment-confirmation condition as satisfied without processing, transferring or holding funds.

## AC-11 Payment remains unconfirmed

**Requirements:** FR-15, FR-20

**Given** a Rental is Reserved and both PaymentConfirmations have not been recorded  
**When** 48 hours pass after the request was accepted  
**Then** the system shall keep the Rental from becoming Active and notify the Customer and Owner that payment confirmation remains incomplete.

## AC-12 Confirm handover and activate rental

**Requirements:** FR-16, FR-20

**Given** a Rental is Reserved and both PaymentConfirmations have been recorded  
**When** the Customer and Owner confirm that the item was handed over  
**Then** the system shall change the Rental to the Active state, record the expected return date and notify both parties.

## AC-13 Exchange rental messages

**Requirements:** FR-14

**Given** a Student is the Customer or Owner participating in a Rental  
**When** the Student sends a Message in that Rental's Conversation  
**Then** the system shall store and deliver the Message to the other participant.

## AC-14 Prevent unauthorised rental messaging

**Requirements:** FR-14

**Given** a Student is not a participant in a Rental  
**When** the Student attempts to access or send a Message in that Rental's Conversation  
**Then** the system shall deny access and shall not store or deliver the Message.

## AC-15 Complete an on-time return

**Requirements:** FR-17, FR-18

**Given** a Rental is Active and the expected return date has not passed  
**When** the Customer and Owner each confirm the return and record the item's condition  
**Then** the system shall create both ReturnConfirmations, change the Rental to Completed and restore the returned quantity to the ItemListing.

## AC-16 Mark an overdue rental

**Requirements:** FR-19, FR-20

**Given** a Rental is Active and both ReturnConfirmations have not been recorded  
**When** the expected return date passes  
**Then** the system shall change the Rental to Overdue, record the overdue status and notify the Customer and Owner within one minute of the scheduled status check.

## AC-17 Complete an overdue rental after return

**Requirements:** FR-17, FR-18, FR-19

**Given** a Rental is Overdue  
**When** the Customer and Owner each confirm the return  
**Then** the system shall change the Rental to Completed and restore the returned quantity to the ItemListing.

## AC-18 Enable ratings only after confirmed return

**Requirements:** FR-21

**Given** both ReturnConfirmations have been recorded and the Rental is Completed  
**When** either participant submits a valid rating of the other participant  
**Then** the system shall store the Rating and associate it with the Rental, author and recipient.

## AC-19 Block premature ratings

**Requirements:** FR-21

**Given** a Rental is not Completed or both ReturnConfirmations are not present  
**When** a participant attempts to submit a Rating  
**Then** the system shall reject the Rating and explain that ratings are available only after confirmed return.

## AC-20 Submit and review a dispute

**Requirements:** FR-22, FR-25

**Given** a Student is signed in and a Rental-related issue has occurred  
**When** the Student submits a Report containing the issue type and description  
**Then** the system shall record the Report as Open and make it available to an Administrator for review and resolution.

## AC-21 Administrative account action

**Requirements:** FR-23

**Given** an Administrator is authorised and an Account issue has been reported  
**When** the Administrator records a justified suspension or deactivation decision  
**Then** the system shall update the Account status and retain the administrative record.

## AC-22 Administrative listing removal

**Requirements:** FR-24

**Given** an Administrator is authorised and an ItemListing violates an applicable rule  
**When** the Administrator removes the ItemListing  
**Then** the system shall stop displaying it in the storefront and retain the removal record.

## AC-23 Generate an administrative report

**Requirements:** FR-26

**Given** an Administrator is signed in and Rental records exist  
**When** the Administrator requests a semester Rental report  
**Then** the system shall produce a downloadable report containing the relevant ItemListing, Customer, Owner and Rental-status information within ten seconds for a dataset of 500 records.

