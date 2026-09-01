1. Concurrent Login Load
•	Stimulus: 50 students attempt to log in simultaneously during peak morning hours.
•	Context: Authentication server connected to UB email system should support an average daily active users of 500.
•	Response: System authenticates all users successfully within 5 seconds.

2. Search Performance for Common Items
•	Stimulus: A renter searches for “fridge.”
•	Context: Marketplace catalog contains over 1,000 items across 10 categories.
•	Response: Search results are displayed within 3 seconds.

3. Transaction Creation Speed
•	Stimulus: A rental record is created when a renter confirms an item.
•	Context: Database holds over 5,000 rental records, which includes item, renter, seller, and due date.
•	Response: System stores and confirms the transaction within 2 seconds.

4. Real Time Chat Responsiveness
•	Stimulus: A chat message is sent between renter and seller.
•	Context: Both users are online, connected via campus Wi Fi.
•	Response: Message is delivered and visible to the recipient in under 1 second.

5. Administrative Report Generation
•	Stimulus: Administrator generates a report of all rentals for the semester.
•	Context: Report includes over 500 records with item details, renter/owner info, and status.
•	Response: Report is produced and ready for download within 10 seconds.

6. Overdue Rental Handling
•	Stimulus: A rental period passes without return confirmation.
•	Context: Transaction due date expired so system checks status at midnight daily.
•	Response: System flags the rental as Overdue and sends notifications to both renter and seller within 1 minute.

7. System Recovery After Downtime
•	Stimulus: Marketplace server restarts after an unexpected outage.
•	Context: 200 active users were logged in before downtime and database contains over 10,000 records.
•	Response: System restores service, reloads user sessions, and resumes pending transactions within 2 minutes of restart.

