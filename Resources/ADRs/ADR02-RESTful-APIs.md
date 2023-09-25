
# ADR 2: Choosing REST and CQRS over GraphQL

On the topic of enabling communication to the back-end APIs, the following were considered:
* Option 1: REST coupled with the CQRS design pattern;
* Option 2: GraphQL.

While both options support the necessitated system architecture, a decision here was necessary as it would impact implementation costs.

## Status

Proposed

## Rationale

The bottom-line difference between both options is that REST provides a well-established standard for implementing traditional APIs while GraphQL offers flexible data querying. As the nature of the technology implies, GraphQL is excellent for the following conditions:
* A: API Requests are mainly data-centric; 
* B: Multiple platforms would like to use the same endpoints but request different information at their whim.

Condition a is not particularly applicable because the business case requirements do not show a significant reliance on data-centric API requests, and in the area where they are (Reporting & Analytics), a segregated solution has been suggested.
With regards to condition b, due to the fact that a PWA front-end is going to be developed (i.e. both the Web Application and the Mobile Application share identical code), there is a single front-end implementation, so the necessity to implement different different endpoint recipients is absent.

In contrast, utilising the REST API combined with CQRS helps build more domain-behaviour oriented API-BL interactions, suitable for covering the business case without introducing additional complexity. The use of CQRS alongside libraries such as MediatR helps streamline the way in which Domain Events can be fired off.

## Decision 

The use of REST + CQRS is proposed.

## Consequences

Positive:
* CQRS allow to separate the load from reads and writes and scale each independently;
* Separating write from read activity leads to more efficient scaling of geographical storage capacity;
* Complex Business Logic or features such as validations can be enclosed within Commands or Queries;
* RESTful APIs are easy to understand and use, it has a rich ecosystem of tools, libraries, and middleware that can simplify development, testing, and documentation.

Negative:
* In some rare scenarios where the PWA may have logic to render different elements based on whether the Web or Mobile application is being used, RESTful APIs may lead to over-fetching or under-fetching of data;
* Although documentation is widely available, CQRS event-sourcing adds an element of complexity to the system.
