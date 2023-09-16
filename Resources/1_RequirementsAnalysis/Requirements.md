# Slides Breakdown – what we need?

-	Requirements | Use Cases | Story Cards | DDD event-storm output | anything else?
-	Archi Characteristics
    -	Availability
    -	Reliability
    -	Security
    -	Scalability
    -	Elasticity
    -	Deploy ability
    -	Performance
    -	View Others
-	Apply Architecture Characteristics within our requirements (why and how). What characteristic will be sacrificed for another etc.
-	Component Identification - System entities
-	Avoid the entity trap – trip manager (too vague)
-	Workflow Approach – Happy paths for specific scenarios (modules that will handle these workflows)  
-	Actor/Action Approach – Main actors (User – What user can do in the system map)
-	Diagram/Detailed Diagram how components/handlers communicate together.
-	Review Judges Criteria
-	Previous submissions (we say this already)
-	explain Why rather than How
-	ADRs (Title, Status, Context, Decision (the why), Consequences)

## Breaking down the requirement slides

![Requirements Breakdown 1](../../Images/Requirements/RequirementsBreakdown1.png)

To comprehensively address the requirement outlined in the brief, it is crucial to break it down into specific entry points and clearly define the payloads we will receive from each of these entry points. This meticulous approach ensures that we understand and manage the data flow effectively. 

Delving deeper into this process:

1. User Registration:

    - Entry Point: The user registration page on the website or mobile app.
    - Payload: User-provided information such as name, email, username, and password.

2. User Login:

    - Entry Point: The login page or API endpoint for authentication.
    - Payload: User credentials, typically comprising a username/email and password.

3. Booking Reservations:

    - Entry Point: Online reservation systems or APIs for flights, hotels, and activities.
    - Payload: Reservation details including dates, times, locations, and confirmation numbers.

4. Profile Updates:

    - Entry Point: User profile settings in the app or website.
    - Payload: User-modified data, such as profile picture, contact information, or travel preferences.

5. Trip/Reservation Creation:

    - Manual Creation
        - Entry Point: A feature allowing users to create and organize trips and reservations.
        - Payload: User-generated trip data, which includes trip names, descriptions, and associated reservations.
    
    - Automated Creation Email 
        - Entry Point: Automated creation of trips or reservations by listening to incomping emails.
        - Payload: System-generated trip data, which includes trip names, descriptions, and associated reservations based off email content.

6. Trip/Reservation Updates:

    - Manual Updates
        - Entry Point: A feature allowing users to manually update trips and reservations.
        - Payload: User-generated trip data, and manually outlined associated reservations based off email content.   
    
    - Automated Email 
        - Entry Point: Automated creation of trips or reservations by listening to incomping emails.
        - Payload: System-generated data and automatically outlined associated reservations based off email content.   

    - Third-Party Integration 
        - Entry Point: Polling of third-party services to scan for updates to reservations.
        - Payload: System-generated data and automatically outlined associated reservations based off polled content.    

7. Itinerary Viewing:

    - Entry Point: The user's dashboard displaying their trip itineraries.
    - Payload: Itinerary information, aggregating reservations for a specific trip.

8. Trip Sharing:

    - Entry Point: The user shares a trip which is the accessed by other user's who can then join the trip
    - Payload: Itinerary information, aggregating reservations for a specific trip.
    
9. Data Analytics:

    - Entry Point: Backend analytics processes that examine user behaviour and preferences.
    - Payload: Analytical data, such as usage statistics, user interactions, and travel patterns.

10. Recommendation Engine:

    - Entry Point: The recommendation engine component of the system.
    - Payload: User data used for analysis, which includes historical travel data, preferences, and behaviour.

By breaking down the requirement into these distinct entry points and their associated payloads, we can ensure that we have a clear understanding of where data enters the system and what information is being processed. This structured approach not only aids in the design and development of the system but also lays the foundation for effective data management, security, and the eventual implementation of analytics and recommendation features.

![Requirements Breakdown 2](../../Images/Requirements/RequirementsBreakdown2.png)
![Requirements Breakdown 3](../../Images/Requirements/RequirementsBreakdown3.png)
![Requirements Breakdown 4](../../Images/Requirements/RequirementsBreakdown4.png)
![Requirements Breakdown 5](../../Images/Requirements/RequirementsBreakdown5.png)