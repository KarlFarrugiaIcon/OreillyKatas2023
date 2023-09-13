
# ADR 2: REST-based API over GraphQL for synchronous requests
This ADR describes the appropriate backend API structure for our project. 


# Rationale
RESTful APIs have become a well-established standard tailored for HTTP-based communication on the web. Notably, a RESTful API represents the highest level of abstraction that refrains from making any assumptions about the underlying system it grants access to. Although GraphQL provides flexibility in querying data, the reliability and familiarity associated with RESTful APIs make them a robust choice for structuring our backend API.

## Decision 
We will use REST over GraphQL our public APIs

## Consequences
Positive:
* RESTful APIs encourage the creation of specific, resource-oriented endpoints. This aligns well with our project's requirements, allowing for fine-grained control over data retrieval and manipulation.
* We can take advantage of HTTP caching mechanisms, which can significantly improve performance.
* It has a rich ecosystem of tools, libraries, and middleware that can simplify development, testing, and documentation.

Negative:
* RESTful APIs might lead us to over-fetching or under-fetching of data.
