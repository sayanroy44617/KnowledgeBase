### Hotel Booking 

#### Functional Requirements
1. Hotel 
   1. Onboarding hotels to the platform
   2. Updates to existing hotels (availability, pricing, etc.)
   3. Booking management (view, cancel, modify)
2. User
   1. Search for hotels based on location, dates, price, etc.
   2. Book Hotels
   3. View and manage bookings
3. Analytics

#### Non-Functional Requirements
- low latency for search and booking operations
- high availability and reliability

#### Constraints
- 500k hotels
- 10M Rooms
- 1000 Rooms / hotel

![URL Shortener System Design](../images/hotel-booking.png)


#### Why Kafka?
Use Kafka when a system needs to handle a continuous stream of events where multiple services must process the same data independently, reliably, and at scale.
It is suited for event-driven architectures that require decoupling, durability, and the ability to replay past events

#### Why Kafka Fits a Hotel Booking System ?
- A booking triggers many actions (payment, room lock, email, analytics). Kafka lets each happen independently instead of chaining API calls.
- Prevents system failures from spreading. If email or analytics is down, bookings still succeed.
- Handles traffic spikes (sales, holidays) by buffering events and processing them gradually.
- Ensures no booking event is lost because events are stored durably.
- Allows replay of past bookings for audits, fixes, or new features.
- Makes it easy to add new services later without changing the booking service.