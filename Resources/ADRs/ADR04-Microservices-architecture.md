
# ADR 4: Microservices Architecture

## Rationale 
Microservices are not a new idea, but a way of applying old programming concepts and best practices to the modern context of distributed and service-oriented architectures. One of these concepts is that software modules should be designed with low coupling and high cohesion, meaning that they should have clear boundaries and responsibilities, and minimal dependencies on each other. This is based on the more fundamental programming principles of separation of concerns, principle of least knowledge, and single responsibility principle.  
Microservices also adopt the principles of Domain Driven Design, which were originally developed to deal with the challenges of large and complex monolithic systems that were divided by technology layers. Domain Driven Design focuses on the business domain and its logic, rather than the technical details of the implementation.  
The advantage of this approach is that it can be applied to any system, regardless of its architecture. Even if we choose to build a modular monolith, we can still use the same principles to design and build its components. The modular structure of the system should reflect the proposed architecture at each layer.

## Decision   
We have opted for the Microservices architecture to design and structure the Road Warrior system

## Consequences  
Positive:
+ It gives us greater scalability and isolation compared to a modular monolith
+ It allows us to use different technologies and programming languages for each service, enabling us to choose the best tool for each specific task
+ independent units that can be developed, deployed, and maintained separately. 

Negative:  
+ Complexity in terms of service discovery, inter-service communication, and data consistency between services.
