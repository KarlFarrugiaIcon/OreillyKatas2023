# ADR 4: Microservices Architecture

The general architecture of the overall system is the starting point of all development work and the catalyst to addressing all non-functional requirements for the system. After careful analysis of the preliminary business case and behavioral requirements, a series of distinct modules were identified from an early stage.

With an array of modules identified and a series of non-functional requirements based around elasticity and scalability, considering Microservices as a baseline architecture for the solution became an obvious option.

This decision is potentially the most impactful one throughout the project as it sets a precedent for the development and infrastructure deployment philosophy to be followed for the entire implementation.

## Rationale 

Microservices are not a new idea, but a way of applying traditional programming concepts and best practices (such as the SOLID Principles) to the modern context of distributed and service-oriented architectures. 

A key principle for developing robust and maintainable software is that modules should be designed with low coupling and high cohesion, having clear boundaries and responsibilities, with little-to-no dependencies on each other. This is based on the more fundamental programming principles of separation of concerns, principle of least knowledge, and single responsibility principle.

Microservices are easily coupled with Domain Driven Design, which were originally developed to deal with the challenges of large and complex monolithic systems that were divided by technology layers. Domain Driven Design focuses on the business domain and its logic, rather than the technical details of the implementation - allowing for easier design of abstract components void of technical constraints.

## Decision   

The Microservices desgin is proposed for the general system architecture across the board.

## Consequences  

Positive:
* Scalability across the board and for individual modules (Elasticity) is easily managed when clearly-distinct service instances are deployed, in comparison to traditional monolithic systems;
* Technological constraints across the board are eliminated, as services are independent and can make use of whatever technologies and elements are necessary without being burdened or bloated with unnecessary implementations;
* Iterative development and deployments are more easily managed when service distinctions are made by design.

Negative:  
* Complexity in terms of infrastructure (i.e. service discovery, inter-service communication) and data consistency between services is increased and requires additional efforts and specific design to ensure reliability.