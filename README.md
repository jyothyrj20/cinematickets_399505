#  Ticket Service Implementation

## Objective
This project implements a **Ticket Service** that processes ticket purchase requests while enforcing strict business rules. It demonstrates clean coding, immutability, exception handling, and integration with external services for **payment processing** and **seat reservations**.

The solution is designed to:
- Validate ticket purchase requests against business rules.  
- Calculate the total payable amount.  
- Reserve the correct number of seats.  
- Reject invalid purchase requests gracefully.  

---

##  Business Rules
1. There are **3 ticket types**:  
   - **Infant** → £0 (no seat allocated, sits on an Adult’s lap).  
   - **Child** → £15 (requires a seat).  
   - **Adult** → £25 (requires a seat).  

2. Ticket purchase rules:  
   - A maximum of **25 tickets** can be purchased per transaction.  
   - **Child and Infant tickets cannot be purchased without at least one Adult ticket.**  
   - Only accounts with **ID > 0** can make a purchase.  

3. Services:  
   - **TicketPaymentService** → Handles external payment.  
   - **SeatReservationService** → Handles seat reservations.  

---

##  Implementation
### Key Classes
- **`TicketServiceImpl`**  
  Implements the `TicketService` interface. Handles validation, calculations, payments, and seat reservations.  

- **`TicketTypeRequest`**  
  Immutable object representing a ticket request (type + quantity).  

- **`InvalidPurchaseException`**  
  Custom exception thrown when a request violates business rules.  

### Flow
1. Validate account ID and ticket rules.  
2. Sum tickets and calculate:  
   - Total payment amount.  
   - Seats to reserve (excluding infants).  
3. Call external services:  
   - `paymentService.makePayment(accountId, totalAmount)`  
   - `reservationService.reserveSeat(accountId, totalSeatsToReserve)`  

---

##  Example
**Input**:  
- 2 Adults  
- 1 Child  
- 1 Infant  

**Result**:  
- Total payable = `(2 × £25) + (1 × £15) = £65`  
- Total seats reserved = `2 Adults + 1 Child = 3`  
- Infant does not pay and does not occupy a seat.  

---

##  Invalid Requests
The service throws an `InvalidPurchaseException` for:  
-  More than 25 tickets requested.  
-  Child/Infant tickets without at least one Adult ticket.  
-  Account ID is null or ≤ 0.  

---

##  How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/ticket-service.git
   cd ticket-service
