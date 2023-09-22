# ADR 15: DDD with CQRS Pattern

## Status  
Approved

## Rationale

It is being proposed to uptake the usage of Domain Driven Design, due to the way it can implement the Domain Boundaries which were being outlined in the event storming phase, and marry it with CQRS since the latter advocates the separation of concerns between commands and queries, enabling us to design and optimise the system for different tasks independently

## Decision

The decision is to take on DDD with CQRS since DDD as a software design approach allows the placing of the domain at the forefront of the development process. 

DDD encourages defining bounded contexts, which are clear boundaries around specific subdomains of the application. Since this has already been done it will not require any tradeoffs or additional work

Moreover through the usage of aggregates and domain models we'll define consistency boundaries and transactional scopes which encapsulates business logic within domain objects. This help ensure data integrity and provide clear access points for modifying data within a bounded context while leading to more maintainable and expressive code.

CQRS on the other hand encourages the separation of the command and query responsibilities. This separation allows us to design our system to excel in both aspects independently.

This also enables horizontal scalability by optimising read and write paths separately. We can scale the read side for high query performance and the write side for handling a large number of commands.

Furthermore, paired with our event-driven architecture CQRS pairs well with Event Sourcing, which records all changes to the application's state as a sequence of events. This can be used for auditing, debugging, and ensuring data consistency.

## Consequences
Positive:

+ Separation of Concerns
+ Scalability
+ Observabilitiy

These are all requirements of our architecture characteristics.

Negative:  

- Complexity
- Potentially Over-engineered solution
- Data synchronisation headaches