
# ADR 2: Choosing REST and CQRS for Server-Side Requests
This ADR describes the appropriate backend API structure for our project. 


## Rationale
In our architectural discussions, we considered REST, GraphQL, and CQRS for server-side requests. REST provides a well-established standard, GraphQL offers flexible data querying, and CQRS enforces a clear separation between commands and queries.

## Decision 
We will use REST + CQRS over GraphQL our public APIs

## Consequences
Positive:
* CQRS allows us to separate the load from reads and writes and scale each independently
* Separating write from read activity leads to more efficient scaling of geographical storage capacity.
* RESTful APIs are easy to understand and use, it has a rich ecosystem of tools, libraries, and middleware that can simplify development, testing, and documentation.

Negative:
* RESTful APIs might lead us to over-fetching or under-fetching of data.
* CQRS event-sourcing may add complexity to the system.
