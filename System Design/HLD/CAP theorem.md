## CAP theorem:



## So what would you prioritize here, consistency or availability?”


1. First: Partition Tolerance is always a given in distributed systems. So in interviews, the real choice is: 
Consistency 🔁 Availability

2. Ask this one question early: 
“If stale data is served for a few seconds, will that break the system or just annoy a user?”

    - If it breaks the system → you need consistency 

    - If it's tolerable → go for availability

3. Real-world analogies 
   - Ticket Booking → Consistency. No double-booked seats. 
   - Inventory Management → Consistency. You can’t sell 1 item to 2 people. 
   - Netflix or Social Media Feed → Availability. Seeing an old post isn’t the end of the world. 
   - Payments, Stocks, Banking → Consistency. One wrong read could cost millions.

4. You can mix both. For example, in a product like Tinder: 
   - Matching system → needs consistency 
   - Profile updates → can afford availability
