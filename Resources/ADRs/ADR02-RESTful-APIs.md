
# ADR 2: Choosing REST and CQRS over GraphQL
This ADR describes the appropriate backend API structure for the solution. 

## Rationale
In our architectural discussions, we considered REST with CQRS vs GraphQL. REST provides a well-established standard, GraphQL offers flexible data querying, and CQRS enforces a clear separation between commands and queries.

## Decision 
We will use REST + CQRS over GraphQL for the public APIs

## Consequences
Positive:
* CQRS allow to separate the load from reads and writes and scale each independently
* Separating write from read activity leads to more efficient scaling of geographical storage capacity.
* RESTful APIs are easy to understand and use, it has a rich ecosystem of tools, libraries, and middleware that can simplify development, testing, and documentation.

Negative:
* RESTful APIs might lead to over-fetching or under-fetching of data.
* CQRS event-sourcing may add complexity to the system.
