
# ADR 3: Distributed Databases and Redis for Global Data Distribution

In addition to some necessitated performance benchmarks, providing a seamless experience for people on the Go can be considered as an implied requirement. The business logic involved in the application is relatively simple, therefore there is only so much that can be done in the ways of the backend design. On the other hand, the architectural elements related to the data infrastructure as well as where and how the data is fetched, can be a significant hinderance to performance if they are neglected.

## Status

Approved

## Rationale 

The application being a travel companion implied that users across the globe will be using the application, and potentially individuals may even access it from different locations in a short timespan.

This meant that considering a distributed-database design, as opposed to a centralised one, offered great promise for contributing towards optimised performance for this specific business case.

As an additional micro-optimisation for improving request performance, pairing data retrieval with a caching service (i.e. Redis), also made sense - especially since there may be several features that involve querying the same elements (such as shifting from one dashboard to the next).

## Decision 

Incorporating distributed databases and Redis into the data segments of the system architecture is proposed.

## Consequences

Positive:
* Distributed databases optimise minimal latency access for users worldwide;
* Managed services for distributed databases (such as Cosmos DB) provide out-of-the-box horizontal scaling features to handle high-throughput workloads;
* Leveraging Redis as a managed service or an in-memory cache is an efficient way to reduce unnecessary requests to the database.

Negative:  
* While performance would be problematic for this scenario, centralised databases are easier to enforce data consistency in comparison to distributed databases, which will require both in-built replication services to be running, and ideally also covered within the code infrastructure;
* Redis may cause synchronisation issues if enough code-safeguards are not put in place.
