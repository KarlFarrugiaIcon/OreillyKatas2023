# ADR 5: Event-Driven Architecture for Event Streaming and Communication between Microservices

The execution of an event-storming exercise to outline the behavioral requirements of the system in addition to the consideration of Microservices and Domain-Driven Design elements as part of the proposed architecture highlighted the need to consider how domain events will be managed.

## Status

Approved

## Rationale

The event-driven approach is based on the idea that microservices communicate with each other by exchanging events. An event is a message that represents something that happened in the system, such as a change in data, a request from a user, or a notification from another service. Events are published to common channels (in this case, the Event Streaming and Queue Infrastructure) where they can be consumed by other interested microservices. This way, microservices can react to events asynchronously and independently, without having to know the details of each other's implementation. 

The need for efficient, robust and secure event streaming was outlined and further emphasised the benefit of considering Event-driven communication over traditional Request-response communication in areas where requests can happen in the background (fire and forget fashion) or outside of a user's thread.

## Decision

The Event-Driven architecture will be used as the foundational approach for handling domain events and communication between Microservices.

## Consequences  
Positive:
* Microservices are achievable because this architecture complements loose-coupling, enabling seamless addition, removal, or updates to individual services without causing disruptions to the entire system;
* It excels in real-time and event stream processing, enabling the handling of continuous event flows for timely decision-making and responses based on real-time data;
* Event-driven architecture supports event sourcing, which preserves the history of system state changes for auditing, debugging, and data consistency.

## Consequences  
Negative:
* Limited control in the areas where events leave system boundaries;
* Complexity is increased because more moving parts are added just to make the system work.
