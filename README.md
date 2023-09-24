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
    - [Breaking down the requirement slides](#breaking-down-the-requirement-slides)
    - [Performance Characteristics](#performance-characteristics)
  - [Technical Constraints](#technical-constraints)
  - [Business Constraints](#business-constraints)
  - [Assumptions](#assumptions)
- [Overall Platform Context](#overall-platform-context)
  - [Component Identification (Boundary Analysis)](#component-identification-boundary-analysis)
- [User Roles](#user-roles)
- [User Experience](#user-experience)
- [Context Diagram](#context-diagram)
  - [High-level Platform Context Diagram](#high-level-platform-context-diagram)
  - [Actor to System Boundary Diagram](#actor-to-system-boundary-diagram)
    - [Deep Dive on System Boundaries](#deep-dive-on-system-boundaries)
      - [Authentication Service](#authentication-service)
      - [Trip Management Service](#trip-management-service)
      - [Email Data Parsing Service](#email-data-parsing-service)
      - [Social Media Service](#social-media-service)
      - [Travel Integration Service](#travel-integration-service)
      - [Notifications Service](#notifications-service)
      - [Reporting & Analytics Service](#reporting--analytics-service)
- [User Interface Mockups](#user-interface-mockups)
  - [Manual Wireframing](#manual-wireframing)
  - [Figma Designs](#figma-designs)
- [Architecture Characteristics](#architecture-characteristics)
  - [Driving Characteristics](#driving-characteristics)
  - [Implicit Characteristics](#implicit-characteristics)
  - [Other Considerations](#other-considerations)
- [Architecture Implementation Styles](#architecture-implementation-styles)
  - [Microservices Architecture](#microservices-architecture)
  - [Event-Driven Architecture](#event-driven-architecture)
  - [Space-Based Architecture](#space-based-architecture)
  - [High Level Combined Architecture](#high-level-combined-architecture)
- [Business Plan](#business-plan)
  - [Freemium Tier](#freemium-tier)
  - [Silver Tier](#silver-tier)
  - [Gold Tier](#gold-tier)
- [MVP Timeline Proposal](#mvp-timeline-proposal)
  - [Delivery Timeline Composition](#delivery-timeline-composition)
- [Identifying Architectural Quanta](#identifying-architectural-quanta)
  - [Kubernetes](#kubernetes)
    - [Container Registry](#container-registry)
  - [Event Bus](#event-bus)
  - [RPA - PowerAutomate](#rpa---power-automate)
  - [Next JS](#next-js)
  - [Cosmos DB](#cosmos-db)
  - [Redis](#redis)
  - [Serverless Functions](#serverless-functions)
  - [Load Balancing](#load-balancing)
    - [Azure Traffic Manager](#azure-traffic-manager)
    - [Azure CDN](#azure-cdn)
    - [Azure Front Door](#azure-front-door) 
  - [Privatelink](#privatelink)
  - [Azure Synapse](#azure-synapse)
- [Overall Architecture and Cost Analysis](#overall-architecture-and-cost-analysis)
  - [MVP 1 - Soft Launch](#mvp-1---road-warrior-soft-launch)
  - [MVP 2 - Integrations](#mvp-2---integrations)
  - [MVP 3 - Reporting and Analytics](#mvp-3---reporting-and-analytics)
  - [MVP 4 - Geographical Distribution](#mvp-4---geographical-distribution)
- [Engineering Practices](#engineering-practices)
  - [Provider Pattern](#provider-pattern)
  - [Domain Driven Design (DDD) with Command Query Responsibility Segregation (CQRS)](#domain-driven-design-ddd-with-command-query-responsibility-segregation-cqrs)
  - [Deployment Pipelines](#deployment-pipelines)
  - [SOLID Principles](#solid-principles)
  - [Unit Tests](#unit-tests)
- [ADRs](#adrs)
- [Resources](#resources)
- [Glossary of Terms](#glossary-of-terms)
  
## Prelude

Road Warrior is a startup poised to revolutionise the travel industry by developing a cutting-edge online trip management platform dedicated to providing travelers with dynamic and manual itinerary management capabilities. This innovative dashboard will empower travelers to effortlessly access and organise all their existing reservations, ensuring a seamless and hassle-free travel experience. Whether users prefer to access it through a web interface or on their mobile devices, this platform will serve as the go-to solution for travelers seeking comprehensive trip management solutions. With this pioneering tool, travelers can look forward to a more organised, convenient, and enjoyable journey, making it the next generation's must-have travel companion. In addition to its user-centric features, this platform will also harness the wealth of data it collects for invaluable reporting purposes. By leveraging this data, travelers will gain insights into their travel patterns, preferences, and spending habits, allowing them to make more informed decisions for future trips. As the platform continues to accumulate user data, it will lay the foundation for a future suggestion engine. 

## Business Case

### Requirements

The provided requirements can be found [here](/Resources/RequirementsAnalysis/Requirements.pptx)

#### Breaking down the requirement slides

![Requirements Breakdown 1](/Images/Requirements/RequirementsBreakdown1.png)

To comprehensively address the requirement outlined in the brief, it is crucial to break it down into specific entry points and clearly define the payloads we will receive from each of these entry points. This meticulous approach ensures that we understand and manage the data flow effectively. 

#### Performance Characteristics

![Requirements Breakdown 2](Images/Requirements/RequirementsBreakdown2.png)

- Updates must be in the app within 5 minutes of an update (better than the competition). This may be tackled by having the latest updates locally and at the core, but have eventual consistency across different regions.
- The system may be composed of 2: 
    - Core services - A stand-alone Kubernetes cluster that is hosted in the region of the business' core user base.
        - Different components have different loads
    - Globally distributed APIs to service read/write operations to their user base.
        - Different loads in different regions
        - Each region will have its own peak, which may be seasonal (summer holidays) or on weekly trends (weekend getaways)

### Technical Constraints

- The startup excels in its business domain but lacks the technical expertise needed for its ambitious projects. Therefore, as the technical supplier, we have a blank slate to start from.

### Business Constraints

- This is a start-up, therefore, funds may be limited and have to be generated from the application itself.

### Assumptions

- Start-up does not have any technical partners
- The start-up will start maturing with the system MVP rollout thereby allowing the system to grow. It is assumed that if the application is not performing well the owners will take a fail-fast approach and may pull the plug. Therefore, the final product will assume that the application will become self-sustaining and cover the operational cost. Both MVP rollout and Cost Analysis will be provided at the end of the literature.

## Overall Platform Context

The event storming process was employed to identify essential "domain events" within a system, where each event represents an action related to a business entity. It's a crucial initial step as these events configure the central artifact for the system. Event storming meetings start with participants noting domain events, foundational for defining business rules. The team wrote down domain events, each represented on an orange sticky note on a virtual whiteboard. This collaborative approach facilitates a comprehensive understanding and mapping of system events for stakeholders.

![Domain Events](Images/DomainBoundaries/DomainEvents.jpg)

Following the identification of domain events, the next step involves pinpointing the commands and users responsible for triggering these events. Commands are actions initiating these events. External actors' commands are explicitly recognised, while some commands originate internally. Post-it notes are arranged to visualise a sequence: actor, command, and event, ensuring a cohesive representation of the system's flow. This step streamlines the understanding of event triggers and user interactions. These commands and domain events are grouped into related aggregates.

![Domain Commands](Images/DomainBoundaries/DomainEventCommands.jpg)

In the final step, post-gathering domain events and defining triggering commands, the focus shifts to automation policies. These policies apply to commands lacking external actors, activated upon the completion of specific domain events, signifying communication ties between bounded contexts. By grouping semantically related aggregates, we define bounded contexts. Visualised in a diagram, these boundaries and event-driven connections take shape.

![Domain Events with Bounded Contexts](Images/DomainBoundaries/DomainEventBoundedContexts.jpg)

### Component Identification (Boundary Analysis)
![Requirements Breakdown 3](Images/Requirements/RequirementsBreakdown3.png)

The solution adheres to a boundary analysis that encompasses several key components to ensure its functionality and effectiveness:

- Trips and Reservations: At the core of the system lies the ability to manage trips and reservations seamlessly. Users can create, update, and view their travel itineraries, which include flights, hotels, and activity reservations.

- Polling Mechanism: To keep information up-to-date, the solution employs a polling mechanism. It regularly checks external sources such as booking platforms for any changes or updates to reservations and synchronises them with the user's itinerary.

- Email Webhooks: For real-time communication and updates, the system integrates with email webhooks. Users receive notifications and updates about their reservations directly in their email, ensuring they stay informed and can make timely adjustments to their plans.

- Data Analytics: The solution incorporates data analytics to derive insights from user interactions, helping to improve user experiences and provide personalised recommendations based on historical travel data.

- Recommendation Engine: Utilizing a recommendation engine, the system offers tailored suggestions to users based on their travel history, preferences, and current bookings, enhancing their travel planning and decision-making process.

- User Authentication: Security is paramount, and the system includes robust user authentication mechanisms to protect user data and ensure only authorised access to accounts and itineraries.

- Third-party Integrations: We integrate with various third-party services and APIs for booking and reservation data, enabling users to seamlessly import and manage their travel information.

By adhering to this boundary analysis, the solution provides a comprehensive and user-centric travel management experience, ensuring efficiency, accuracy, and user satisfaction throughout the journey planning process.

## User Roles

The identified actors and their actions are as follows:

| Actor          | Actions |
| -------------- | ------- |
| Customer (Authenticated)      | - Registers on the platform<br /> - Logs in the platform<br /> - Consent to email forwarding<br /> - View upcoming trips<br /> - Manage upcoming trips<br /> - View trip reservations<br /> - Manage trip reservations<br /> - Receives notifications regarding upcoming trips<br /> - View personalised analytics<br /> - Request for help from agency<br /> - Share trip details on preferred social media platform<br /> - Share trip details with platform<br /> - Share trip details with anonymous user<br /> |
| Customer (Not Authenticated) | - View shared Trip Summary|
| System Admin   | - Registers on the platform<br /> - Logs in the platform<br /> - Add multi-lingual translations <br /> - View regional analystics<br />  |

## User Experience

Delving deeper into the process outlined in [Breaking down the Requirements](#breaking-down-the-requirement-slides):

1. User Registration:

    - Entry Point: The user registration page on the website or mobile app.
    - Payload: User-provided information such as name, email, username, and password.

      ![Alt text](./Images/UserFlows/image.png)

2. User Login:

    - Entry Point: The login page or API endpoint for authentication.
    - Payload: User credentials, typically comprising a username/email and password.

      ![Alt text](./Images/UserFlows/image-1.png)    

3. Profile Updates:

    - Entry Point: User profile settings in the app or website.
    - Payload: User-modified data, such as profile picture, contact information, or travel preferences.

4. Trip/Reservation Creation:

    - Manual Creation
    <br/>**Original Requirement**: '_Customers should be able to add, update, or delete existing reservations manually as well_'

        - Entry Point: A feature allowing users to create and organise trips and reservations.
        - Payload: User-generated trip data, which includes trip names, descriptions, and associated reservations.

          ![Alt text](./Images/UserFlows/image-3.png)

    - Automated Creation Email
    <br/>**Original Requirement**: '_Poll email looking for travel-related emails. Filter and whitelist certain emails_'

        - Entry Point: Automated creation of trips or reservations by listening to incoming emails.
        - Payload: System-generated trip data, which includes trip names, descriptions, and associated reservations based on email content.

          ![Alt text](./Images/UserFlows/image-4.png)

      - Third-Party Creation
      <br/>**Original Requirement**: '_The system must interface with the agency’s existing airline, hotel, and car rental interface system to update travel details_'

        - Entry Point: Online reservation systems or APIs for flights, hotels, and activities.
        - Payload: Reservation details including dates, times, locations, and confirmation numbers.

        ![Alt text](./Images/UserFlows/image-5.png)

5. Trip/Reservation Deletion:

    - Manual Delete
     <br/>**Original Requirement**: '_Customers should be able to add, update, or delete existing reservations manually as well_'

        - Entry Point: A feature allowing users to manually delete trips and reservations.
        - Payload: User-generated trip data, and manually outlined associated reservations based on email content.   
    
          ![Alt text](./Images/UserFlows/image-6.png)

    - Automated Email
     <br/>**Original Requirement**: '_Poll email looking for travel-related emails. Filter and whitelist certain emails_'

        - Entry Point: Automated deletion of trips or reservations by listening to incoming emails.
        - Payload: System-generated data and automatically outlined associated reservations based on email content.   

          ![Alt text](./Images/UserFlows/image-7.png)

    - Third-Party Integration
    <br/>**Original Requirement**: '_The system must interface with the agency’s existing airline, hotel, and car rental interface system to update travel details_'

        - Entry Point: Polling of third-party services to scan for removed reservations.
        - Payload: System-generated data and automatically outlined associated reservations based on polled content.    

          ![Alt text](./Images/UserFlows/image-8.png)

6. Trip/Reservation Updates:

    - Manual Updates
     <br/>**Original Requirement**: '_Customers should be able to add, update, or delete existing reservations manually as well_'

        - Entry Point: A feature allowing users to manually update trips and reservations.
        - Payload: User-generated trip data, and manually outlined associated reservations based on email content.   
    
          ![Alt text](./Images/UserFlows/image-9.png)

    - Automated Email
    <br/>**Original Requirement**: '_Poll email looking for travel-related emails. Filter and whitelist certain emails_'
      
        - Entry Point: Automated updates of trips or reservations by listening to incoming emails.
        - Payload: System-generated data and automatically outlined associated reservations based on email content.   
        
          ![Alt text](./Images/UserFlows/image-10.png)

    - Third-Party Integration
    <br/>**Original Requirement**: '_The system must interface with the agency’s existing airline, hotel, and car rental interface system to update travel details_'

        - Entry Point: Polling of third-party services to scan for updates to reservations.
        - Payload: System-generated data and automatically outlined associated reservations based on polled content.    

          ![Alt text](./Images/UserFlows/image-11.png)

7. Itinerary Viewing:
<br/>**Original Requirement**: '_Items in the dashboard should be able to be grouped by trip, and once the trip is complete, the items should automatically be removed from the dashboard_'

    - Entry Point: The user's dashboard displaying their trip itineraries.
    - Payload: Itinerary information, aggregating reservations for a specific trip.

8. Trip Sharing:
<br/>**Original Requirement**: '_Users should also be able to share their trip information by interfacing with standard social media sites or allowing targeted people to view your trip_'

    - Entry Point: The user shares a trip which is accessed by other users who can then join the trip
    - Payload: Itinerary information, aggregating reservations for a specific trip.

      ![Alt text](./Images/UserFlows/image-2.png)
    
10. Data Analytics:

    - User Analytics
    <br/>**Original Requirement**: '_Provide end-of-year summary reports for users with a wide range of metrics about their travel usage_'

      - Entry Point: Backend analytics processes that examine specific user's behaviour and preferences.
      - Payload: Analytical data, such as usage statistics, user interactions, and travel patterns.

      ![Alt Text](./Images/UserFlows/user-analytics-flow.PNG)

    - System Analytics
    <br/>**Original Requirement**: '_Road Warrior gathers analytical data from users trips for various purposes - travel trends, locations, airline and hotel vendor preferences, cancellation and update frequency, and so on_'

      - Entry Point: Backend analytics processes that examine users' behaviours across different countries and regions.
      - Payload: Analytical data, such as usage statistics, user interactions, travel patterns, and trends across different regions.
        
      ![Alt Text](./Images/UserFlows/System-Analytics-flow.PNG)

11. Recommendation Engine:

    - Entry Point: The recommendation engine component of the system.
    - Payload: User data used for analysis, which includes historical travel data, preferences, and behaviour.

      ![Alt Text](./Images/UserFlows/recommendations-flow.PNG)

By breaking down the requirement into these distinct flows with entry points and their associated payloads, we can ensure that we have a clear understanding of where data enters the system and what information is being processed. This structured approach not only aids in the design and development of the system but also lays the foundation for effective data management, security, and the eventual implementation of analytics and recommendation features.

## Context Diagram

To help us visualise the system we use the actors and components that were outlined in previous sections and drafted the following context diagrams.

### High-level Platform Context Diagram

The below context diagram provides a high-level introduction to actions that the different User types can perform on the application. The abstractions of the different components (or services) responsible for handling all possible actions triggered by users or external interfaces.

![High-level Platform Context Diagram](/Images/SystemActors/HighLevelContextDiagram.png)

### Actor to System Boundary Diagram

The below actor-to-system boundary diagram expands on the detail provided by the High-level Platform Context Diagram, by describing communication methods between different components (now broken down into services, providers, and external tools) as well as their utilisation of infrastructure components such as databases and messaging/event channels to realise the features offered by the system.

![Actor To System Boundary](/Images/SystemActors/Actor-To-System-Boundary.png)

#### Deep Dive on System Boundaries 

The following section provides a description of the interactions with the neighbouring systems module is subject to (scoped at a Microservice level) and its corresponding interactions with neighbouring systems.

##### Common Characteristics for All System Boundaries
The following are some characteristics that are present for every module and thus are described prior to the service-specific descriptions.

- The entry-point for all requests going downstream from the Client-facing applications is always through an Application Gateway which acts as a security mechanism for validating that requests towards the backend services come from an accepted origin.
- Each Microservice is provisioned within a Kubernetes Cluster and is designed to scale horizontally based on the load on the system. A minimum of one service instance will be made available in different regions. 
- A load balancer will be configured within the Kubernetes Cluster to handle redirection of requests based on proximity and availability.
- Standard database operations occur against a data center with the closest proximity to the originating request. To support eventual consistency, the updated/deleted records are replicated across the distributed database instances in near real-time.

##### Authentication Service

The Authentication Service is responsible for facilitating authentication mechanisms through username and password or social media Single Sign-On (SSO).

Through the use of the provider pattern, the authentication service leverages abstraction to provide a default implementation of the standard authentication operations and then uses the provider pattern to differentiate between the concrete implementation of the internal username/password implementation or external social media SSO APIs.

![Alt text](./Images/DomainBoundaries/AuthenticationService.png)

##### Trip Management Service

The Trip Management Service is responsible for allowing users to view and manage trips and reservations. 

The Trip Management Service is subscribed to the Event Streaming Infrastructure to listen for incoming events from the Travel Integration and the Email Data Parsing Service for new records and/or changes to existing Trip or Reservation Records.

If changes to locally persisted Trip or Reservation records are made, messages are published through the Event Streaming infrastructure to notify interested parties of said changes.

When use of the 'Share trip to social media' feature is done, a message is published to the Queue Infrastructure, which is being listened to by the Social Media Sharing Service.

![Alt text](./Images/DomainBoundaries/TripManagementService.png)

##### Email Data Parsing Service

The Email Data Parsing Service is responsible for collecting trip and reservation data from Email sources. 

As indicated in other areas of the solution documentation, Users will be given instructions on how to create rules for forwarding travel-related emails to the service mailbox. Emails received by the service mailbox are observed by an External Automation Tool (such as Power Automate) and are then published to the Event Streaming infrastructure as JSON objects detailing the email data. The Email Data Parsing service is Subscribed to the Event Streaming Infrastructure so that it can consume and break down email data objects, and then publish them through the same Event Streaming Infrastructure so that the Trip Management Service can ultimately persist them.

![Alt text](./Images/DomainBoundaries/EmailDataParsingService.png)

##### Social Media Sharing Service

The Social Media Sharing Service is responsible for sharing content on social media platforms. 

It receives prompts from the Trip Management Service via a Queue infrastructure, and utilises external Social Media providers for Authentication and Sharing to successfully share content to said platforms.

![Alt text](./Images/DomainBoundaries/SocialMediaSharingService.png)

##### Travel Integration Service

The Travel Integration Service is responsible for collecting trip and reservation data from Travel Agency Integrations. 

It subscribes via AMQP (Advanced Message Queuing Protocol) to configure external travel agencies, processes the data, and publishes messages to a queue. Abstraction features are used to cover baseline processing operations and then use the provider pattern to integrate with different external travel agency integration services.

Successfully parsed incoming records are then subsequently published to the Event Streaming Infrastructure for further processing and local persistence by the Trip Management Service.

![Alt text](./Images/DomainBoundaries/TravelIntegrationService.png)

##### Notifications Service

The Notifications Service is responsible for pushing notifications to the Public-facing applications.

It subscribes via AMQP to the Event Streaming Infrastructure, listening in to messages concerning new or adjusted Trips/Reservations coming in from the Trip Management Service, and then subsequently raises notifications to active users on the Web or Mobile users with the PWA installed via a Publish/Subscribe mechanism.

![Alt text](./Images/DomainBoundaries/NotificationService.png)

##### Reporting & Analytics Service

The Reporting & Analytics Service is used to generate reports and store data in a format suitable for reporting within the data warehouse.

The service is subscribed to the Event Streaming Infrastructure for updates stemming from the Trip Management Service so that changes can be propagated to the data warehouse (and stored in an unstructured way). It uses restful APIs to communicate with an external reporting & analytics service (such as PowerBI), to generate and embed reports and statistics. The external reporting & analytics service is configured to read from the data warehouse, and can also be consumed via an External Tool (such as PowerBI Desktop), for system admins to access reporting for the entire platform.

![Alt text](./Images/DomainBoundaries/ReportingAndAnalyticsService.png)

## User Interface Mockups

Mock-ups are essential in the development process of the solution since it allows the team to visualise and conceptualise the idea. It also allows us to take a user-centered approach that aligns with the requirements. 

### Manual Prototyping

The first approach for prototyping is the traditional pen and paper with the results being show cased hereunder.

![roadwarriorManual](/Images/ManualWireframing/manualsketches.gif)

### Figma Prototyping

After the manual prototyping, the next flow was to do a Figma design of the solution with the results being shown hereunder.

![roadwarrior](Images/UI/flow.gif)

https://github.com/KarlFarrugiaIcon/OreillyKatas2023/assets/91567864/99460df7-7392-4e82-ba34-daa91e1c5cab

## Architecture Characteristics

This section takes into consideration how the architecture is to be split using the [Developer to Architect Architecture Resource](https://developertoarchitect.com/resources.html). This is intended to outline key architectural attributes we deem essential for a successful system implementation.

![ArchitectureCharacteristics](/Images/ArchitecturalCharacteristics/ArchitectureCharacteristics.PNG)

### Driving Characteristics

| Preferred | Characteristics | Reason |
| ----------- | ----------- | ----------- |
| [X] | Scalability | The system needs to be highly scalable since it needs to grow to accommodate increased demand and workload. This scalability is essential in the context of the solution, as travel-related services often experience fluctuations in user traffic, especially during peak seasons or special events. Whether it's a sudden surge in users making reservations or an uptick in concurrent users accessing their itineraries, the system can efficiently allocate additional resources to handle the increased load. This scalability ensures that users experience uninterrupted service and swift response times, regardless of the system's level of demand.  |
| [X] | Elasticity | Elasticity takes the concept of scalability a step further by not only allowing the system to grow but also contract when demand decreases. The solution needs to be designed with elasticity in mind, enabling it to automatically adjust its resource allocation based on real-time demand. For instance, during periods of lower user activity, the system can scale down to conserve resources, reducing operational costs. Conversely, when demand surges, it can quickly scale up to meet the increased load. This elasticity ensures cost-efficiency and optimal resource utilisation, making the solution adaptable and financially sustainable over time. |
| [] | Data Integrity & Consistency | Ensuring the integrity and consistency of data is paramount in this system. There is a need to implement robust data validation mechanisms, error-handling processes, and transaction management to prevent data corruption or discrepancies. By maintaining data integrity and consistency, we guarantee that users can rely on accurate information throughout their travel planning and management processes. |
| [] | Abstraction | Abstraction is a foundational element of the system's architecture. It allows us to shield users and developers from unnecessary complexities by presenting simplified and user-friendly interfaces. By abstracting the underlying technical intricacies, we enhance usability and reduce the complexities of integrating future applications of similar types of existing implementations. |
| [] | Availability |  The solution has to be built with high availability in mind due to the requirement of a maximum of 5 minutes of downtime per month. There is a need to employ redundancy, failover mechanisms, and disaster recovery strategies to minimise downtime and ensure that users can access their travel information 24/7. Availability is critical in the travel industry, where users may require access to their itineraries and bookings at any time. |
| [X] | Performance |  Performance optimisation is a key focus in the architectural design. Therefore, the system needs to employ efficient algorithms, caching mechanisms, and load balancing to deliver fast response times and smooth user interactions. Whether users are viewing their itineraries or receiving real-time recommendations, the system will need to consistently deliver high-performance results. |
| [] | Interoperability | Interoperability to facilitate seamless communication with external systems and services. This needs to adhere to industry standards and implement standardised data exchange protocols to ensure that our platform can integrate with various third-party providers, booking systems, and travel-related services. This interoperability enhances the user experience by offering comprehensive access to travel-related resources. |

### Implicit Characteristics

| Characteristics | Reason |
| ----------- | ----------- |
| Feasibility / Cost | This implicit characteristic comes as a result of the start-up nature of the client and revolves around the financial aspects of a software project. Feasibility analysis assesses whether the project is financially viable and if the expected benefits outweigh the costs. It also considers factors like budget constraints, resource availability, and potential return on investment. Addressing this may require some early-on concessions when designing MVPs which will eventually be made less cost effective and more efficient once the solution becomes self-sustaining. |
| Maintainability | Maintainability refers to the software's ease of modification, enhancement, and long-term sustainability. Implicitly, it underscores the importance of writing clean, modular, and well-documented code. It involves practices such as code refactoring, version control, and adherence to coding standards such as abstraction. A maintainable software system is more cost-effective to update and extend over time, reducing the risk of technical debt and ensuring that the software remains adaptable to changing requirements. |
| Observability | Observability is focused on a software system's ability to provide insights into its behavior, performance, and issues. It involves implementing logging, monitoring, and error-tracking mechanisms. Observability allows developers and operators to gain visibility into the system's internal workings, making it easier to diagnose and resolve problems, optimise performance, and ensure that the software meets its operational objectives. Implicitly, observability emphasises proactive system health management and continuous improvement through data-driven insights. |

### Other Considerations 

Ensuring availability in different global regions is a complex yet critical aspect of modern digital services. It involves deploying redundant infrastructure, global distribution of data, and leveraging Content Delivery Networks (CDNs) to minimise latency and downtime. Factors such as geographical diversity, local regulations, and varying network conditions must be considered. Achieving high availability means that users, regardless of their location, can access services reliably and consistently. This global approach to availability not only enhances user experiences but also strengthens disaster recovery capabilities, ensuring that services remain resilient even in the face of regional disruptions.

## Architecture Implementation Styles

Based on the Characteristics the chosen architecture is based on microservices, event-driven and space-based architecture.
![ArchitecutreImplementation](/Images//ArchitecturalCharacteristics/ArchitectureStyles.PNG)

### Microservices Architecture
The system will adopt a Microservices Architecture to promote modularity and scalability. Different components of the system, such as user management, reservation handling, and recommendation generation, will be developed as independent microservices. Each microservice will have its own database and will communicate with others through the event bus. This approach allows for agile development, easy maintenance, and the ability to scale specific services independently to meet varying demands. For example, during peak travel booking seasons, we can allocate more resources to the reservation microservice while keeping other services unaffected.

While there might be a performance trade-off associated with microservices, it's feasible to mitigate this drawback by incorporating strategies such as caching, scaling, and database sharding.

[ADR 4 - Microservice Architecture](/Resources/ADRs/ADR04-Microservices-architecture.md)

### Event-Driven Architecture
Event-driven architecture will be integral to the system's real-time capabilities. Events, such as user actions (booking a flight, changing an itinerary) or external updates (flight delays, hotel availability), will trigger asynchronous messages that various components can subscribe to and act upon. For instance, when a user adds a new reservation, it generates an event that updates the user's itinerary and triggers the recommendation engine to suggest relevant activities or accommodations. This decoupled and event-driven approach ensures that the system remains responsive, scalable, and capable of handling real-time data updates seamlessly.

[ADR 5 - Event Driven Architecture](/Resources/ADRs/ADR05-Event-driven-architecture.md)

### Space-Based Architecture
Space-based architecture will be employed for managing distributed, in-memory data caches and ensuring high availability and low-latency access to frequently accessed data. This architecture allows us to store and retrieve data in a distributed and fault-tolerant manner, which is crucial for a system handling real-time travel information. For example, we can use a space-based architecture for caching frequently accessed itinerary data, ensuring that users can quickly access their travel plans regardless of the data's physical location. This architecture also supports data consistency and synchronization across multiple regions for enhanced availability and performance.

[ADR 14 - Space-Based Architecture](/Resources/ADRs/ADR14-Space-Based-architecture.md)

### High-Level Combined Architecture

This leads to the following high-level solution approach

![SolutionApproach](/Images/ArchitecturalCharacteristics/ArchitectureDiagram.png)

## Business Plan

The business plan revolves around strategic partnerships, software development, and infrastructure resources to provide a user-friendly platform with personalised recommendations for travelers. This involves ongoing investments in personnel, software development tools, marketing, and customer support. The revenue streams are diverse, encompassing subscription models, future transaction fees, advertising partnerships, and premium features, which help offset operational costs and drive profitability. Road Warrior is committed to enhancing user experience and fostering strong customer relationships as part of its ongoing strategy, this ensures a sustainable and successful business.

![Business Model Plan](/Images/BusinessPlan/businessmodel.png)

### Freemium Tier

1. Single Inbox Integration: Users in the freemium tier can connect one email inbox to import and organise their travel-related information, such as flight bookings, hotel reservations, and itineraries.
2. Basic Itinerary Management: They can create and manage basic travel itineraries, including flight details, accommodation, and activities. Users can view and edit their trips within the platform.
Notification Alerts: Receive basic email notifications for trip updates, such as flight delays or gate changes, directly within the platform.
3. Calendar Integration: Sync their travel itineraries with their preferred calendar application (e.g., Google Calendar or Outlook).

### Silver Tier

1. Multiple Inbox Integration: Silver-tier users can connect and manage multiple email inboxes, making it easier to centralise travel-related information from various accounts.
2. Trip Sharing: Share trip itineraries with friends, family, or colleagues. Collaboratively plan and coordinate travel with others, and allow others to view and comment on shared trips.
3. Advanced Notification Alerts: Receive real-time updates for travel-related events, such as flight status changes, gate information, or delays. Customise notification preferences for added convenience.
4. Customisable Itineraries: Enjoy more advanced itinerary customization options, including adding notes, reminders, and personal preferences for each trip.

### Gold Tier

1. Unlimited Inbox Integration: Gold-tier subscribers can connect an unlimited number of email inboxes, allowing for comprehensive and centralised trip management across multiple email accounts.
2. Premium Recommendations: Receive personalised travel recommendations based on user preferences and past travel history. These recommendations can include suggested destinations, accommodations, and activities.
3. Priority Customer Support: Access priority customer support with faster response times and dedicated assistance for any inquiries or issues.
4. Exclusive Discounts: Enjoy exclusive discounts and offers on travel bookings, such as flights, hotels, or rental cars, through partnerships with travel providers.
5. Advanced Reporting and Analytics: Gain access to detailed trip analytics, including travel expenses, trip duration, and historical travel trends, helping users make more informed travel decisions.
6. Premium Content: Access premium travel content, such as destination guides, travel tips, and insider recommendations, to enhance the travel planning experience.
7. Ad-Free Experience: Navigate the platform without any advertisements or sponsored content for an uninterrupted user experience.

## MVP Timeline Proposal

The platform roadmap that has been drafted takes into consideration the infancy of the enterprise and has therefore been designed in such a way that focuses on introducing streams of revenue as soon as possible to cover necessary funding for the undertaking of this project.

### Delivery Timeline Composition

Four named MVPs are being proposed:
*  MVP 1: Road Warrior Soft-Launch - As the namesake implies, this MVP will involve launching the product with just the essential, barebones features, suitable enough to introduce the potential of the product to the market. The majority of requirements specified in the initial spec are covered completely, with other less critical requirements being delivered in part or planned for launch in a future MVP. This MVP will help establish 'Road Warrior' in the traveling organisation app market and potentially even introduce investment opportunities. The inclusion of lightweight advertisements in the barebones version of the application will also introduce a new and immediate stream of revenue, scaling based on the number of users (as will running costs, as a matter of fact).

*  MVP 2: Shared Dashboards and Integrations - Introduce features that support collaboration/sharing among authenticated users, expanding the social elements of the application. At this point, all baseline requirements from the original specification barring reporting & analytics are implemented to some degree. Expanding integrations with users' mailboxes, and additional booking agencies will also increase traffic on the application, increasing traffic and introducing new opportunities for further investments.

*  MVP 3: Subscription Model, Analytics & Reporting - By the time that the development and planned delivery of MVP3 is underway, the project should have established an audience (this will be assisted through relevant marketing efforts). A larger audience in addition to increased features (and complexity of said features), means that computing costs will increase just as well. Advertisements will cover a portion of these running costs, however, to offer a more seamless experience as well as more advanced (resource-intensive) features, a subscription model will be released.

*  MVP 4: Expand Covered Services - This is the last "planned" MVP for the product. Here, the platform will undergo horizontal diversification in the services and data it offers by covering attractions and taxis.

*  MVP N+: At this point in time, the project will be in maintenance mode. Bugfixes and performance adjustments will be issued as needed, while new features, covered services and booking agency integrations will be incrementally increased based on community feedback

![image](./Images/MVPs/MVPRoadmap.jpg)

## Identifying Architectural Quanta

The following section outlines the different components which make up the architecture. While this section outlines concrete implementations to a specific cloud provider the solution will still be abstracted in a way that we'll create a vendor-agnostic solution without the risk of a vendor lock-in.

### Kubernetes

Kubernetes plays a pivotal role in load-balancing the core services of the system, ensuring that they remain highly available, scalable, and responsive to user requests. This is done by:

- Simplifying service deployment of core services as containers within a cluster. Each service is encapsulated in a container, making it easy to manage and scale independently.

- Ability to use Replica Sets to maintain a specified number of replicas (containers) for each core service. This ensures that even if one container fails, a new one is automatically spawned, maintaining the desired level of service availability.

- The usage of the built-in service discovery mechanisms which enables load balancing to ensure that incoming requests are distributed evenly across the available service replicas.

- Management of entry points through the usage of ingress controllers for external traffic into the cluster. These can route incoming requests to the appropriate core service based on defined rules, such as domain names or URL paths.

- Kubernetes enables automatic scaling of core services based on predefined metrics such as CPU utilisation. When traffic increases, Kubernetes can dynamically spin up additional service replicas to handle the load, ensuring optimal performance.

#### Container Registry

The container registry is an essential infrastructure component for Kubernetes. It centralises image management, version control, and distribution, promoting efficient and secure software delivery.

[ADR 10 - Load Balancing](/Resources/ADRs/ADR10-Load-balancing-Core-Services.md)

### Event Bus

The event bus allows different parts of the solution to exchange information in a loosely coupled manner. It enables components or services to publish events and subscribe to events of interest. This approach was chosen since it is widely used in event-driven architectures, microservices, and distributed systems to facilitate seamless communication and data exchange among various system elements.

[ADR 4 - Microservice Architecture](/Resources/ADRs/ADR04-Microservices-architecture.md)

[ADR 5 - Event Driven Architecture](/Resources/ADRs/ADR05-Event-driven-architecture.md)

[ADR 14 - Space-Based Architecture](/Resources/ADRs/ADR14-Space-Based-architecture.md)

### RPA - Power Automate

Given that the solution will be listening to a Road Warrior's owned mailbox it will be possible for the solution to implement RPA by having a 'when email received' trigger on the mailbox. This action would then allow the core services to work on the parsed email data.

[ADR 8 - Polling vs Webhooks with Email Forwarding Rule](/Resources//ADRs/ADR08-Polling-vs-InboxWebhooksWithEmailForwardingRule.md)

### Next JS as a PWA

Given that the system needs to be performant Next.js was chosen due to its support for Server-Side Rendering (SSR) and Progressive Web App (PWA) capabilities.

SSR offers several advantages namely 
**improved SEO** and **faster initial page load** which are crucial for the app to obtain adoption with the user base.

PWAs offer features that allow the application to be much more accessible due to **offline support** which allows for browsing in areas of limited internet, **app experience and packing** which facilitates publishing to mobile stores, **caching strategies** which allow the storage of assets and data on the client's device to ensure fast load times on subsequent visits.

[ADR 1 - Progressive Web App](/Resources/ADRs/ADR01-Progressive-web-app.md)

[ADR 6 - SSR](/Resources/ADRs/ADR06-Server-side-rendering.md)

### Cosmos DB

CosmosDB is the backbone of the app's data management strategy. With its globally distributed, multi-model database service, CosmosDB enables us to seamlessly handle vast amounts of data, provide low-latency access to users worldwide, and ensure high availability and scalability. Its support for various data models, including document, key-value, graph, and column family, offers the flexibility needed to store and query diverse types of data efficiently. CosmosDB's built-in global distribution, automatic scaling, and robust consistency options align perfectly with the app's requirements for data resilience, real-time updates, and responsive performance. It's the foundational layer that empowers the app to deliver a seamless and data-rich user experience.

[ADR 3 - Cosmos DB and Redis for Global Data Distribution](/Resources/ADRs/ADR03-Data-Distribution.md)

[ADR 9 - Cosmos DB](/Resources/ADRs/ADR09-CosmosDB-Consistency.md)

[ADR 12 - Distribution of Data Globally](/Resources/ADRs/ADR12-Distributing-Data-Globally.md)

### Redis

Redis plays a pivotal role in enhancing the speed and efficiency of the app. As an in-memory data store, Redis excels at caching frequently accessed data, reducing database load, and significantly improving response times for users. Its support for data structures like strings, sets, and hashes makes it versatile for various application needs, such as session management, real-time analytics, and queuing. With Redis, the app can deliver fast data retrieval and processing, ensuring a snappy and highly responsive user experience. It's a key component that enhances the overall performance and scalability of the application.

[ADR 3 - Cosmos DB and Redis for Global Data Distribution](/Resources/ADRs/ADR03-Data-Distribution.md)

[ADR 13 - Usage of Serverless Functions with Redis Over APIs](/Resources/ADRs/ADR13-Usage-of-Serverless-Functions-with-Redis-Over-APIs.md)

### Serverless functions

Serverless functions enable the application to execute code in a highly efficient and cost-effective manner. By leveraging serverless computing platforms like Azure Functions, the solution will be able to run code in response to events or API requests without the need to manage servers or infrastructure. This approach enables rapid development, automatic scaling, and optimal resource utilization. These functions provide the solution with the agility and scalability needed to deliver a seamless and responsive user experience while minimising operational overhead and costs.

[ADR 13 - Usage of Serverless Functions with Redis Over APIs](/Resources/ADRs/ADR13-Usage-of-Serverless-Functions-with-Redis-Over-APIs.md)

### Load Balancing

Load balancing is a critical component of the app's infrastructure. This is achieved by leveraging Azure's suite of services to ensure optimal performance and availability.

#### Azure Traffic Manager

Azure Traffic Manager intelligently distributes user traffic across multiple data centers based on pre-configured geographical rules.

#### Azure CDN

Azure CDN accelerates content delivery by caching and serving static assets from edge locations worldwide, reducing latency for users.

#### Azure Front Door

Azure Front Door acts as a global entry point, combining security and load balancing to direct traffic to the nearest available backend service.

[ADR 10 - Load Balancing Core Services](/Resources/ADRs/ADR10-Load-balancing-Core-Services.md)


[ADR 12 - Distribution of Data Globally](/Resources/ADRs/ADR12-Distributing-Data-Globally.md)

### Privatelink

In order to improve security, reliability, and performance for the main cluster and the geographically dispersed API endpoints, the solution will employ the usage of private links. This is a service that enables secure and private communication between the application and services, like databases, storage, and other resources, without traversing the public internet. 

This approach will be utilised to improve:

- **Security** since this approach ensures that data transfer between the application and cloud services remains within the private network, isolated from the public internet. This significantly reduces the attack surface and minimises the risk of unauthorised access or data breaches.

- **Data Privacy** through the establishment of a private, dedicated connection, ensuring that sensitive data does not leave the private network during transit. This is critical for maintaining the privacy and confidentiality of user information, travel itineraries, and other travel-related data.

- **Improved Performance** by eliminating the need for data to traverse the public internet.

Overall this approach is expected to create an isolated environment for the application's backbone thereby reducing exposure to external threats and ensuring that our application's dependencies are accessible only through a private, secure channel.

### Azure Synapse

Azure Synapse serves as the backbone of the app's data analytics and warehousing capabilities. With its powerful data integration, transformation, and analytics tools, Azure Synapse enables the solution to harness the full potential of the collected data. It seamlessly integrates with various data sources and provides a unified platform for data storage, processing, and visualization. Whether it's running complex analytical queries, creating data pipelines, or generating actionable insights, Azure Synapse empowers the solution to make data-driven decisions and deliver a richer, more informed user experience.

## Overall Architecture and Cost Analysis

Having gone over the [MVP Timeline Proposal](#mvp-timeline-proposal) and identified the core components that will make the system in [Identifying Architectural Quanta](#identifying-architectural-quanta) we will start to outline how the solution will physically be built vis-a-vis the MVP roll out and the expected cost at each phase of the architecture. Azure has been used as an example platform to reference specific managed services and calculate a baseline cost. As previously mentioned the system is to be built in an abstract way that allows all managed services to be swapped out to any other Cloud managed services. Azure will however be used for us to be able to come up with a base price for the platform.

Throughout the technical build-up, we constantly kept in mind the following requirements: 

- 2 million active users per week
- 15 million total users within the system
- Maximum of 5 minutes downtime per month
- Updates must be within the app within 5 minutes
- Response time should be 800ms and the first contentful paint under 1.4 seconds.

### MVP 1 - Road Warrior Soft-Launch

Given that Road Warrior is a start-up it is critical to ensure a cost-effective MVP rollout that does not cripple the start-up. Therefore, we will concentrate on delivering a lean and focused version of our product. Utilising cloud services, and taking a scale-as-you-go approach, we will optimise development costs. Our design will be minimalistic yet functional, and we will follow an agile development approach for rapid iteration based on user feedback. We'll continuously monitor costs and performance to make data-driven decisions. This approach will enable us to validate our concept while effectively managing our startup's financial resources.

To this end the first MVP is a bare-bones deployment consisting of:
- Standard Kubernetes Cluster
- Standard Container Registry
- Standard Event Grid
- Standard Cosmos DB
- Premium App Services for PWA and API
- General Purpose Storage Account
- Azure DNS

While this is not the most performant for the forecasted user base, we do not expect a huge amount of traffic in the initial rollout either. Therefore, we foresee this to be viable in the beginning. The below diagram depicts the infrastructure set up at this point

![Technical Architecture MVP 1](Images/TechnicalImplementation/MVP1-Katas.png)

#### Cost Analysis

| Service | Specifications | Cost |
| ----------- | ----------- | ----------- |
| Azure Kubernetes Service (AKS) | 1 Linux D4a v4 Node (no reserved instances) with S4 - 32GB of OS Disk | $238.06 |
| Azure Container Registry | Standard | $20.00 |
| Azure Cosmos DB | Serverless with 200GB of storage | $50.25 |
| Event Grid | Standard - Event Grid Namespace (Assuming up to 5 million monthly operations) | $1.80 |
| Storage Account | General Purpose v2 | $23.88 |
| App Service | Premium V2 (P1V2) to be used by API and PWA | $146.00 |
| Azure DNS | Zone 1 Public DNS | $0.90 |
| IP Addresses | Global ARM 1 Static IP | $16.06 |
| | | $496.95 |

Our proposed initial commitment to Road Warriors is $496.95 per month. This infrastructure is expected to handle a good workload but not the expected 2 million monthly active users. However, we do not expect to have this workload in the initial phases, notwithstanding the expectations if the system metrics show strain it will be possible for us to alleviate the cloud's potential and scale accordingly. 

### MVP 2 - Integrations

This iteration will continue on building on MVP 1 and start to add core functionality through integrations with third-party vendors and users' mailboxes. This means that apart from further alleviating the usage of our existing Event Grid we also need to start utilising RPA for the **when mail received** trigger. It would also be expected that the initial load from MVP 1 will now be strained and therefore the infrastructure will be scaled up. At this moment we do not believe that committing to reserved instances will be beneficial since the system would still be undergoing rapid growth.

The MVP 2 iteration will see the following changes:

- Upgraded Kubernetes Cluster
- Upgraded Cosmos DB
- Upgraded App Service instance
- RPA - Power Automate

![Technical Architecture MVP 2](Images/TechnicalImplementation/MVP2-Katas.png)

#### Cost Analysis

| Service | Specifications | Cost |
| ----------- | ----------- | ----------- |
| Azure Kubernetes Service (AKS) | 1 Linux D8a v4 Node (no reserved instances) with S4 - 32GB of OS Disk | $401.58 |
| Azure Container Registry | Standard | $20.00 |
| Azure Cosmos DB | Autoscale Provisioned Throughput with 200GB of storage | $137.60 |
| Event Grid | Standard - Event Grid Namespace  (Assuming up to 10 million monthly operations)  | $5.40 |
| Storage Account | General Purpose v2 | $23.88 |
| App Service | Premium V2 (P2V2) to be used by API and PWA | $292.00 |
| Azure DNS | Zone 1 Public DNS | $0.90 |
| IP Addresses | Global ARM 1 Static IP | $16.06 |
| Power Automate | 1 Standard User | $15.00 | 
| | | $912.41 |

The cost at this stage is expected to go up to $912.41 per month. While this is almost double the cost of MVP 1 it can be noted that the core services' Cluster, Database, and front-facing App Service have also been significantly upgraded. These upgrades are due to the additional load that the third-party integration will start introducing and with the expectations that the system would have started to generate traction and more users are onboarding.

### MVP 3 - Reporting and Analytics

This iteration focuses mainly on the Analytics and Reporting aspect of the system which will be expected to feature greatly in the application's forecasted growth. At this point, we are also assuming that the amount of active users per week is starting to approach the 2 million mark. Therefore, this MVP iteration will see the following changes:

- Upgraded Cluster
- Azure Synapse Analytics
- Apache Spark
- Power BI 

![Technical Architecture MVP 3](Images/TechnicalImplementation/MVP3-Katas.png)

#### Cost Analysis

| Service | Specifications | Cost |
| ----------- | ----------- | ----------- |
| Azure Kubernetes Service (AKS) | 2 Linux D8a v4 Node (no reserved instances) with S4 - 32GB of OS Disk | $728.62 |
| Azure Container Registry | Standard | $20.00 |
| Azure Cosmos DB | Autoscale Provisioned Throughput with 200GB of storage | $137.60 |
| Event Grid | Standard - Event Grid Namespace  (Assuming up to 20 million monthly operations)  | $11.40 |
| Storage Account | General Purpose v2 | $23.88 |
| App Service | Premium V2 (P2V2) to be used by API and PWA | $292.00 |
| Azure DNS | Zone 1 Public DNS | $0.90 |
| IP Addresses | Global ARM 1 Static IP | $16.06 |
| Power Automate | 1 Standard User | $15.00 | 
| Azure Synapse | Compute Optimised Gen2 with 100 DWU Blocks and a 3 year reserve instance |  $397.30 | 
| Apache Spark Pool | Small Memory Optimised (4 vCores with 32 GB) | $166.92 |
| Power BI | 1 Premium User | $20.00 | 
| | | $1,829.64 |

The cost has once more doubled from MVP 2 to MVP 3 with the new forecasted cost being at $1,829.64 per month. However, this iteration, apart from more upgrades to the cluster starts setting the foundation of the analytics engine. While this is costly it is also an essential part of the application and has therefore started to feature.

### MVP 4 - Geographical Distribution 

The final main iteration will consist of geographical expansion through the replication of Cosmos DB via geographical distribution and the usage of better load-balancing techniques. This iteration will also used to gather usage metric data to commit to reserved instances for 3 years to bring down the cost of infrastructure. While this means that Road Warriors is committed to 3 years with the same minimum cluster size we are assuming that the start-up has now stabilised and has prospects of more growth going forward. To this end, MVP 4 will focus on geographical distribution and load-balancing by adding:

  - Upgraded Cluster (with reserved instances)
  - Cosmos DB Geographical Distribution
  - Azure CDN
  - Azure Front Door
  - Azure Traffic Manager

This leads to the below final overall architecture

![Technical Architecture MVP 4](Images/TechnicalImplementation/MVP4-Katas.png)

#### Cost Analysis

| Service | Specifications | Cost |
| ----------- | ----------- | ----------- |
| Azure Kubernetes Service (AKS) | 2 Linux E16-8as v5 Node (3 year reserved instances) with S4 - 32GB of OS Disk | $774.02 |
| Azure Container Registry | Standard | $20.00 |
| Azure Cosmos DB | Autoscale Provisioned Throughput with 200GB of storage with availability in West Europe, East US, East Asia, and Southeast Asia and a maximum of 2000 Requests per second | $500.40 |
| Event Grid | Standard - Event Grid Namespace  (Assuming up to 50 million monthly events)  | $29.40 |
| Storage Account | General Purpose v2 | $23.88 |
| App Service | Premium V2 (P1V2) to be used by PWA in 4 regions | $584.00 |
| Serverless Functions | Consumption assuming up to 100,000,000 requests per month in 4 regions | $157.60 |
| Traffic Manager | 10,000,000 DNS queries per month | $5.40 |
| Azure CDN | Static Data in 4 zones | $ 3.66 |
| Azure Front Door | Entry point for PWA | $35.51 |
| Azure Redis Cache | Standard C2 Cache in 4 regions | $654.08 |
| Azure DNS | Zone 1 Public DNS | $0.90 |
| IP Addresses | Global ARM 1 Static IP | $16.06 |
| Power Automate | 1 Standard User | $15.00 | 
| Azure Synapse | Compute Optimised Gen2 with 100 DWU Blocks and a 3 year reserve instance |  $397.30 | 
| Apache Spark Pool | Small Memory Optimised (4 vCores with 32 GB) | $166.92 |
| Power BI | 1 Premium User | $20.00 | 
| | | $3,404.13 |

While once more we are seeing a steep cost when compared to MVP 3 with the new monthly cost going to $3,404.13 per month we have managed to make our application more accessible and responsive in different parts of the globe. 
This is critical since the nature of the application makes it required to be performant globally since even if the user base is focused in a specific country, the same users will largely be consuming the contents of the application while actively on a trip.

The final cost of $3,404.13 per month should not be taken as a fixed number since we would continuously continue to monitor the application to see if we need to scale up or down. Such scaling will have an effect on the cost with respect to the scaling direction.

## Engineering Practices

The following are some software engineering practices that will be adhered to during the undertaking of the project:

### Provider Pattern
A design pattern that is used to abstract the creation of objects or services. This pattern decouples client code from the specific implementation and is commonly used in dependency injection and inversion of control.

This pattern will be used thoroughly within the solution in areas where common code can be used to cover features that are fed inputs from different sources that need to undergo the same business logic, as is the case with supporting different SSO authentication providers, different travel agency integrations, and so on.

[ADR 7 - Provider Pattern](/Resources/ADRs/ADR07-Provider-pattern.md)

### Domain Driven Design (DDD) with Command Query Responsibility Segregation (CQRS)

Given the usage of domain boundary analysis in the event storming phase it comes naturally that the solution will adopt DDD with CQRS as an engineering pattern. This combination allows the building of complex, scalable, and maintainable software systems. All of these are identified as key architectural characteristics that the solution requires as key pillars of its foundation.

This methodology empowers the creation of a shared understanding of the complex travel management domain and the crafting of a software solution that truly aligns with the real-world intricacies of travel, reservations, and user interactions. In this context, DDD is not just an engineering strategy but allows the creation of a user-centric travel management platform.

By combining CQRS with DDD, we achieve a robust and flexible architecture. The write side of CQRS aligns well with DDD's focus on modeling the domain, encapsulating business logic, and enforcing consistency.

The read side of CQRS complements DDD by providing optimised query paths, making it easier to retrieve data in a format that matches the user's needs.

Events can be leveraged to communicate changes between bounded contexts, facilitating loose coupling and flexibility in our application's architecture.

[ADR 15 - DDD with CQRS Pattern](/Resources/ADRs/ADR15-DDD-with-CQRS-pattern.md)

### Deployment Pipelines
Deployment pipelines refer to an automated series of steps for deploying changes to the product. This is in line with the chosen idea to produce MVPs that increment on top of each other with new features. It helps ensure consistent and reliable software delivery, void of human errors caused by mistakes in the deployment process.

This practice is supplemented by CI/CD (Continuous Integration/Continuous Deployment):
- CI: Frequent integration of code changes into a shared repository.
- CD: Automated deployment of code to production or other environments.

The combination of Deployment Pipelines and CI/CD practices promotes rapid development, testing, and deployment of software.

### SOLID Principles
The SOLID principles are a series of guidelines for writing maintainable and extensible code. When followed, they help improve code design, readability, and maintainability.

1. **Single Responsibility Principle (SRP):** A class should have only one reason to change, meaning it should have a single responsibility or job.
2. **Open/Closed Principle (OCP):** Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification, encouraging the use of inheritance or interfaces for adding new functionality.
3. **Liskov Substitution Principle (LSP):** Subtypes or derived classes should be substitutable for their base types without altering the correctness of the program.
4. **Interface Segregation Principle (ISP):** Clients should not be forced to depend on interfaces they do not use; it promotes the creation of smaller, more focused interfaces.
5. **Dependency Inversion Principle (DIP):** High-level modules should not depend on low-level modules, both should depend on abstractions, and abstractions should not depend on details; it encourages the use of interfaces or abstract classes to decouple components.

### Unit Tests
Small, isolated tests that validate the behavior of individual code units (e.g., modifications of trips/reservations). Unit tests help ensure that each piece of code works correctly in isolation and contribute towards consistent code quality assurance.

## ADRS

[ADR 1 - Progressive Web App](/Resources/ADRs/ADR01-Progressive-web-app.md)

[ADR 2 - Choosing REST and CQRS over GraphQL](/Resources/ADRs/ADR02-RESTful-APIs.md)

[ADR 3 - Cosmos DB and Redis for Global Data Distribution](/Resources/ADRs/ADR03-Data-Distribution.md)

[ADR 4 - Microservice Architecture](/Resources/ADRs/ADR04-Microservices-architecture.md)

[ADR 5 - Event Driven Architecture](/Resources/ADRs/ADR05-Event-driven-architecture.md)

[ADR 6 - SSR](/Resources/ADRs/ADR06-Server-side-rendering.md)

[ADR 7 - Provider Pattern](/Resources/ADRs/ADR07-Provider-pattern.md)

[ADR 8 - Polling vs Webhooks with Email Forwarding Rule](/Resources//ADRs/ADR08-Polling-vs-InboxWebhooksWithEmailForwardingRule.md)

[ADR 9 - Cosmos DB](/Resources/ADRs/ADR09-CosmosDB-Consistency.md)

[ADR 10 - Load Balancing of Core Services](/Resources/ADRs/ADR10-Load-balancing-Core-Services.md)

[ADR 11 - Segregation of Core Services and Reader APIs](/Resources/ADRs/ADR11-Segregation-of-Core-Services-and-Reader-APIs.md)

[ADR 12 - Distribution of Data Globally](/Resources/ADRs/ADR12-Distributing-Data-Globally.md)

[ADR 13 - Usage of Serverless Functions with Redis Over APIs](/Resources/ADRs/ADR13-Usage-of-Serverless-Functions-with-Redis-Over-APIs.md)

[ADR 14 - Space-Based Architecture](/Resources/ADRs/ADR14-Space-Based-architecture.md)

[ADR 15 - DDD with CQRS Pattern](/Resources/ADRs/ADR15-DDD-with-CQRS-pattern.md)

## Resources

[Introducing event storming](http://ziobrando.blogspot.com/2013/11/introducing-event-storming.html)

[Fundamentals of Software Architecture](https://learning.oreilly.com/library/view/fundamentals-of-software/9781492043447/)

[Software Architecture Patterns](https://learning.oreilly.com/library/view/software-architecture-patterns/9781098134280/)

[Software Architecture: The Hard Parts](https://learning.oreilly.com/library/view/software-architecture-the/9781492086888/)

[Developer to Architect Architecture Resources](https://developertoarchitect.com/resources.html)

[Strategyzer Business Model Canvas](https://www.strategyzer.com/library/the-business-model-canvas)

## Glossary of Terms

- MVP (Minimum Viable Product): An initial version of a product with just enough features to satisfy early adopters and gather feedback for future development.
- PWA (Progressive Web App): A web application that provides a native app-like experience, including offline access and push notifications, while being delivered through a web browser.
- API (Application Programming Interface): A set of rules and protocols that allows different software applications to communicate and interact with each other.
- Globally Distributed APIs and Databases: APIs and databases that are spread across multiple geographical locations to improve performance, reliability, and redundancy.
- Load Balancing: The practice of distributing network traffic or workloads across multiple servers or resources to ensure optimal utilization and prevent overloading.
- Eventual Consistency: A consistency model in distributed systems where, after a period of time, all replicas of data will be consistent, even if there are temporary inconsistencies.
- Client-side component: A software component or module that runs and executes within a user's web browser or client application.
- Server-side component: A software component or module that runs on a remote server and serves requests from client-side components or users.
- Abstract and Complete implementation: In software design, abstract implementations provide only the necessary functionality, while complete implementations include all the required features and details.
- HTTPS (Hypertext Transfer Protocol Secure): A secure version of HTTP that encrypts data exchanged between a client and a server, ensuring data confidentiality and integrity.
- RESTful API (Representational State Transfer): An architectural style for designing networked applications that use HTTP methods and adhere to specific constraints, making them scalable and easy to understand.
- AMQP (Advanced Message Queuing Protocol): A messaging protocol that enables communication between distributed systems, emphasizing message queuing, routing, and reliability.
- Publish/Subscribe Pattern: A messaging pattern where senders (publishers) of messages don't specifically target receivers (subscribers); instead, messages are broadcast to all interested subscribers based on topics or interests.
- Domain-Driven Design (DDD): A software development approach that focuses on creating a shared understanding of the problem domain (the real-world subject area relevant to the software) among technical and non-technical stakeholders. 
- Command Query Responsibility Segregation (CQRS): An architectural pattern that separates the handling of commands (write operations) from queries (read operations). 
