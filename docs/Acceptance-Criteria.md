# Remmogo Acceptance Criteria

## AC-01 Successful Rental Request

**Scenario:** Customer successfully requests to rent an available item  
**Related requirements:** FR-06, FR-07, FR-11

**Given** the Customer has a valid UB account, is signed in, and the selected item is available in the marketplace  
**When** the Customer selects the item, clicks **Rent**, specifies the rental period, and submits the rental request  
**Then** the system shall create a pending rental transaction, reserve the item, record the Customer, Owner, item details and rental period, and notify the Owner of the rental request.

## AC-02 Payment Confirmation and Rental Activation

**Scenario:** Customer and Owner confirm payment for a rental  
**Related requirement:** FR-08

**Given** a pending rental transaction exists and the Customer has confirmed that payment was made outside Remmogo  
**When** the Owner confirms that payment has been received  
**Then** the system shall record both payment confirmations, update the rental transaction status, set the rental due date, update item availability, and send confirmation notifications to both parties.

## AC-03 Prevent Duplicate Booking of an Unavailable Item

**Scenario:** Customer attempts to rent an already rented or reserved item  
**Related requirements:** FR-07, FR-11

**Given** the Customer is signed in and the selected item is already rented or reserved  
**When** the Customer searches for the item or attempts to submit a rental request  
**Then** the item shall not appear as available, the system shall prevent the duplicate booking, and the existing rental transaction shall remain unchanged.

## AC-04 Handling Payment Not Confirmed

**Scenario:** Rental remains pending when payment is not confirmed  
**Related requirement:** FR-08

**Given** a rental request has been created but payment confirmation has not been received from both parties  
**When** 48 hours pass without both payment confirmations  
**Then** the system shall keep the rental transaction pending, notify the Customer and Owner, and prevent the rental from becoming active.

## AC-05 Return and Overdue Management

**Scenario:** System handles a late return  
**Related requirements:** FR-09, FR-11

**Given** an active rental has reached its due date and the item has not been confirmed as returned  
**When** the return deadline passes  
**Then** the system shall mark the rental as overdue, notify the Customer and Owner, and record the overdue status in the rental history.
