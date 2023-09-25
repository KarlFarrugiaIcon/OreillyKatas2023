# ADR 7: Provider Pattern

## Status  
Approved

## Rationale
To connect multiple agencies and to implement multiple authentication providers in The Road Warrior, ensuring both extensibility and consistency is paramount. Recognising this need, the Provider Pattern is considered following a thorough evaluation of behavioural design patterns.

## Decision
The provider pattern will be implemented in the noted areas as well as other areas that may require similar interaction.

## Consequences
Positive:
* Allows for easy customization and configuration of the service or component for different clients, without affecting the core logic or functionality;
* Enables reuse of existing providers or creation of new ones, as long as they adhere to the interface contract;
* Facilitates testing and debugging, as the providers can be mocked or stubbed for isolation and verification.

Negative:  
* It introduces complexity in the design and implementation of the interface
* As the application evolves, there's a risk of creating an excessive number of providers for different features or components;
