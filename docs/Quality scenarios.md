# Remmogo Quality Scenarios

## QS-01 Concurrent Login Load

**Quality attribute:** Performance  
**Stimulus:** 50 Students attempt to sign in simultaneously during peak morning hours.  
**Context:** The authentication service is operating normally and the system supports an average of 500 daily active users.  
**Response:** The system authenticates valid users successfully.  
**Measure:** All authentication responses are returned within 5 seconds.

## QS-02 Search Performance for Common Items

**Quality attribute:** Performance  
**Stimulus:** A Customer searches for “fridge.”  
**Context:** The Remmogo catalogue contains more than 1,000 items across 10 categories.  
**Response:** The system searches the catalogue and displays matching available items.  
**Measure:** Search results are displayed within 3 seconds.

## QS-03 Rental Transaction Creation Speed

**Quality attribute:** Performance  
**Stimulus:** A Customer submits a valid request to rent an available item.  
**Context:** The database contains more than 5,000 rental records, including item, Customer, Owner and due-date information.  
**Response:** The system creates and confirms the pending rental transaction.  
**Measure:** The transaction is stored and confirmed within 2 seconds.

## QS-04 Real-Time Chat Responsiveness

**Quality attribute:** Performance  
**Stimulus:** A chat message is sent between a Customer and Owner.  
**Context:** Both users are online and connected through the campus Wi-Fi network.  
**Response:** The system stores and delivers the message to the recipient.  
**Measure:** The message is visible to the recipient within 1 second.

## QS-05 Administrative Report Generation

**Quality attribute:** Performance  
**Stimulus:** An Administrator requests a report of all rental transactions for the semester.  
**Context:** The report contains more than 500 records with item details, Customer and Owner information, and rental status.  
**Response:** The system generates the report and makes it available for download.  
**Measure:** The report is ready within 10 seconds.

## QS-06 Overdue Rental Handling

**Quality attribute:** Reliability  
**Stimulus:** A rental period passes without return confirmation.  
**Context:** The transaction due date has expired and the system performs its scheduled daily status check.  
**Response:** The system marks the rental transaction as Overdue and notifies the Customer and Owner.  
**Measure:** The status is updated and notifications are issued within 1 minute of the scheduled check.

## QS-07 System Recovery After Downtime

**Quality attribute:** Recoverability  
**Stimulus:** The Remmogo server restarts after an unexpected outage.  
**Context:** Before the outage, 200 users were signed in and the database contained more than 10,000 records.  
**Response:** The system restores service, reloads valid user sessions and resumes pending transactions.  
**Measure:** Normal service is restored within 2 minutes.
