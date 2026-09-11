# Remmogo Business Rules

## Accounts and roles

**BR-01 - UB email registration**  
Only users with a valid University of Botswana email address may register on the system.

**BR-02 - User roles**  
Users are assigned roles such as Student, Customer, Owner and Administrator. A Student may act as a Customer or Owner depending on the rental transaction.

## Item listings

**BR-03 - Item listing ownership**  
Only registered Owners may create item listings.

**BR-04 - Item availability**  
An item may be rented only when its available quantity is greater than zero.

## Rental transactions

**BR-05 - Payment confirmation**  
A rental transaction is considered valid only after both the Customer and Owner have confirmed payment.

**BR-06 - Return date**  
Every rental transaction must have a defined return date.

**BR-07 - Rating eligibility**  
Only completed rental transactions, for which both parties have confirmed the item’s return, may be rated.

## Administration

**BR-08 - Administrative reporting**  
Administrators may generate reports about user activity, rental transactions and disputes.
