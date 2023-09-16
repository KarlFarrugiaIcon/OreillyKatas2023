# O'Reilly Architecture Katas 2023

Team Members:  
- Karl Farrugia
- Romario Grech
- Nathan Povo
- Steve Stellini
- Muhammad Hamid Mughal

## Contents
- [Prelude](#prelude)
- [Business Case](#business-case)
  - [Requirements](#requirements)
  - [Technical Constraints](#technical-constraints)
  - [Business Constraints](#business-constraints)
  - [Assumptions](#assumptions)
- [Architecture Characteristics](#architecture-characteristics)
  - [Driving Characteristics](#driving-characteristics)
  - [Implicit Characteristics](#implicit-characteristics)
  - [Other Considerations](#other-considerations)
- [Architecture Implementation Styles](#architecture-implementation-styles)
  - [Solution Approach](#solution-approach)
- [Non-functional Requirements](#non-functional-requirements)
- [Overall Platform Context](#overall-platform-context)
- [User Experience](#user-experience)
- [Assumptions](#assumptions)
- [User Roles](#user-roles)
- [Actor Diagram](#actor-diagram)
- [Identifying Architectural Quanta](#identifying-architectural-quanta)
- [Overall Architecture](#overall-architecture)
- [Platform Roadmap](#platform-roadmap)
- [Engineering Practices](#engineering-practices)
- [ADRs](#adrs)
- [Resources](#resources)
- [Our 3rd party integrations](#our-3rd-party-integrations)
  
## Prelude

Road Warrior is a startup which is poised to revolutionise the travel industry by developing a cutting-edge online trip management platform dedicated to providing travelers with dynamic and manual itinerary management capabilities. This innovative dashboard will empower travelers to effortlessly access and organise all their existing reservations, ensuring a seamless and hassle-free travel experience. Whether users prefer to access it through a web interface or on their mobile devices, this platform will serve as the go-to solution for travelers seeking comprehensive trip management solutions. With this pioneering tool, travelers can look forward to a more organised, convenient, and enjoyable journey, making it the next generation's must-have travel companion. In addition to its user-centric features, this platform will also harness the wealth of data it collects for invaluable reporting purposes. By leveraging this data, travelers will gain insights into their travel patterns, preferences, and spending habits, allowing them to make more informed decisions for future trips. As the platform continues to accumulate user data, it will lay the foundation for a future suggestion engine. 

## Business Case

### Requirements

The provided requirements can be found [here](/Resources/1_RequirementsAnalysis/Requirements.pptx) and the planned breakdown can be seen [here](/Resources/1_RequirementsAnalysis/Requirements.md)

### Technical Constraints

- The startup excels in its business domain but lacks the technical expertise needed for its ambitious projects. Therefore, as technical supplier we have a blank slate to start from.

### Business Constraints

- This is a start up therefore funds may be limited and have to be generated from the application itself.

### Assumptions

- Start up does not have any technical partners
- The start up will start maturing with the system MVP roll out thereby allowing the system to grow. It is assumed that if the application is not performing well the owners will take a fail fast approach and may pull the plug. Therefore, the final product while will take into consideration the cost will also assume that the application is now self sustaining. 

## Architecture Characteristics

This section takes into consideration how the architecture is to be split using the [Developer to Architect Architecture Resource](https://developertoarchitect.com/resources.html). This is intend to outline key architectural attributes we deem essential for a successful system implementation.

![ArchitectureCharacteristics](/Images/ArchitecturalCharacteristics/ArchitectureCharacteristics.PNG)

### Driving Characteristics

| Preferred | Characteristics | Reason |
| ----------- | ----------- | ----------- |
| [X] | Scalability | The system needs to be highly scalable since it needs to grow to accommodate increased demand and workload. This scalability is essential in the context of the solution, as travel-related services often experience fluctuations in user traffic, especially during peak seasons or special events. Whether it's a sudden surge in users making reservations or an uptick in concurrent users accessing their itineraries, the system can efficiently allocate additional resources to handle the increased load. This scalability ensures that users experience uninterrupted service and swift response times, regardless of the system's level of demand.  |
| [X] | Elasticity | Elasticity takes the concept of scalability a step further by not only allowing the system to grow but also contract when demand decreases. The solution needs to be designed with elasticity in mind, enabling it to automatically adjust its resource allocation based on real-time demand. For instance, during periods of lower user activity, the system can scale down to conserve resources, reducing operational costs. Conversely, when demand surges, it can quickly scale up to meet the increased load. This elasticity ensures cost-efficiency and optimal resource utilization, making our solution adaptable and financially sustainable over time. |
| [] | Data Integrity & Consistency | Ensuring the integrity and consistency of data is paramount in this system. There is a need to implement robust data validation mechanisms, error handling processes, and transaction management to prevent data corruption or discrepancies. By maintaining data integrity and consistency, we guarantee that users can rely on accurate information throughout their travel planning and management processes. |
| [] | Abstraction | Abstraction is a foundational element of the system's architecture. It allows us to shield users and developers from unnecessary complexities by presenting simplified and user-friendly interfaces. By abstracting the underlying technical intricacies, we enhance usability and reduce the complexities of integrating future applications of similar types of existing implementations. |
| [] | Availability |  The solution has to be built with high availability in mind due to the requirement of a maximum of 5 minutes downtime per month. There is the need to employ redundancy, failover mechanisms, and disaster recovery strategies to minimize downtime and ensure that users can access their travel information 24/7. Availability is critical in the travel industry, where users may require access to their itineraries and bookings at any time. |
| [X] | Performance |  Performance optimisation is a key focus in of the architectural design. Therefore, the system needs to employ efficient algorithms, caching mechanisms, and load balancing to deliver fast response times and smooth user interactions. Whether users are viewing their itineraries or receiving real-time recommendations, the system will need to consistently delivers high-performance results. |
| [] | Interoperability | Interoperability to facilitate seamless communication with external systems and services. This needs to adhere to industry standards and implement standardised data exchange protocols to ensure that our platform can integrate with various third-party providers, booking systems, and travel-related services. This interoperability enhances the user experience by offering comprehensive access to travel-related resources. |

### Implicit Characteristics

| Characteristics | Reason |
| ----------- | ----------- |
| Feasibility / Cost | it's a start up |
| Maintainability | |
| Observabilitiy | |

### Other Considerations 

Availability in different global regions

## Architecture Implementation Styles

Based off the Characteristics the chosen architecture is based off microservices, event-driven and space-based architecture.
![ArchitecutreImplementation](/Images//ArchitecturalCharacteristics/ArchitectureStyles.PNG)

### Solution Approach

![SolutionApproach](/Images/ArchitecturalCharacteristics/ArchitectureDiagram.png)

## Non-functional Requirements

*The team performed an analysis of business requirements (listed down requirement by requirement, and their observations), NFR for each one, and then in the main section just submitted a list of NFRs encompassing all of them. The team did some additional research on the background of the business entities involved, to finetune the NFRs for their needs (e.g. the business case concerned non-profit organisations, so it made sense to focus on feasibility all throughout the system).*

## Overall Platform Context

The event storming process was employed to identify essential "domain events" within a system, where each event represents an action related to a business entity. It's a crucial initial step as these events configure the central artifact for the system. Event storming meetings start with participants noting domain events, foundational for defining business rules. The team wrote down domain events, each represented on an orange sticky note on a virtual whiteboard. This collaborative approach facilitates comprehensive understanding and mapping of system events for stakeholders.

_To be updated_

![Domain Events](Images/DomainEvents.jpg)

Following the identification of domain events, the next step involves pinpointing the commands and users responsible for triggering these events. Commands are actions initiating these events. External actors' commands are explicitly recognized, while some commands originate internally. Post-it notes are arranged to visualize a sequence: actor, command, and event, ensuring a cohesive representation of the system's flow. This step streamlines the understanding of event triggers and user interactions.   These commands and domain events are grouped into related aggregates.

_To be updated_

![Domain Commands](Images/DomainEventCommands.jpg)

In the final step, post-gathering domain events and defining triggering commands, the focus shifts to automation policies. These policies apply to commands lacking external actors, activated upon the completion of specific domain events, signifying communication ties between bounded contexts. By grouping semantically related aggregates, we define bounded contexts. Visualized in a diagram, these boundaries and event-driven connections take shape.

_Insert final diagram_

## User Experience

*Section name a bit misleading, here they submitted linear (happy path) processes of end users going through the process.* 

## Assumptions

*Some general assumptions to take everything into context, and give more sense to the narrative.* 

## User Roles

*System user roles.* 

## Actor Diagram

*Actor Diagram*

## Identifying Architectural Quanta

*Listing all architecture components and supporting services. For each one, they submitted an architecture diagram, depicting essentially from front-end to back, including external sevices, DB, API/Messaging/Communication style, etcetera.* 

## Overall Architecture

*Logical and Physical view of the Entire System. The Physical View shows the whole picture of each component shown in Identifying Architectural Quanta.* 

![KatasTechnical Architecture](Images/architecture.png)

## Platform Roadmap

*Section detailing an ideal roadmap, with their suggestion top provide an MVP and then ultimately expand the project in the long term.* 

## Engineering Practices

*References to engineering techniques or design patterns they are considering as part of their implementation. Basic descriptions on each one.* 

## ADRS

*ADRs*

## User Interface Mockups

_Insert brief write-up_

![roadwarrior](Images/UI/flow.gif)

![roadwarriorvideo](Images/UI/flow.mp4)
## Resources

*Links to relevant resources.* 

[Introducing event storming](http://ziobrando.blogspot.com/2013/11/introducing-event-storming.html)

### Our 3rd party integrations

*how/what do they support (this will dictate potentially some of our decisions re. the 5 minute)* 
