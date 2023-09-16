
# ADR 5: Event-Driven Architecture for Event Stream and Communication

## Status  
Proposed

## Rationale
The event-driven approach is based on the idea that microservices communicate with each other by exchanging events. An event is a message that represents something that happened in the system, such as a change in data, a request from a user, or a notification from another service. Events are published to a common channel, such as a message broker or an event bus, where they can be consumed by other interested microservices. This way, microservices can react to events asynchronously and independently, without having to know the details of each other's implementation. 

In our project's architectural planning, we identified the need for efficient event stream processing and communication. To address this requirement, we evaluated different architectural approaches, including Request-Response and Event-Driven Architectures. The choice of architecture would have a significant impact on how events are handled, propagated, and processed within the system

## Decision   
we have decided to adopt an Event-Driven Architecture as the foundational approach for handling event streams and communication within the Road Warrior system

## Consequences  
Positive:
+ This ensures loose coupling among our microservices, enabling seamless addition, removal, or updates to individual services without causing disruptions to the entire system.
+ It excels in real-time and event stream processing, enabling the handling of continuous event flows for timely decision-making and responses based on real-time data
+ Event-driven architecture supports event sourcing, which preserves the history of system state changes for auditing, debugging, and data consistency.

## Consequences  
Negative:
+ Lack of control
+ Complexity
