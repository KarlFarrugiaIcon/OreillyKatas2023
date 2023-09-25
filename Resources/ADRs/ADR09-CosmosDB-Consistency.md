# ADR 9: Choice of "Eventual Consistency" for Distributed Databases

## Status

Approved

## Rationale

### Technological Considerations

When a user performs a write operation, we aim to provide a responsive and efficient experience. Relying on local storage to show the latest data aligns with our goal of minimizing latency for write operations. This approach facilitates prioratising the speed of write operations by not waiting for the database to replicate data immediately.

### Cost Implications

The project places a strong emphasis on cost-effectiveness, thus efforts to optimise resource utilisation are taken into strong consideration in the architectural design. Eventual consistency is the most cost-effective option among the five well-defined consistency levels offered by managed distributed services (such as Azure Cosmos DB). It consumes fewer resources compared to stronger consistency levels, making it an ideal choice for a cost-conscious approach.

### Project Requirements

The project's requirements favour high availability and partition tolerance, which are essential elements of the CAP theorem. The team acknowledges that read consistency may lag behind the real data due to our choice of eventual consistency. However, this trade-off is acceptable in this context, as the primary focus is to ensure that write operations are fast and readily available to users.

## Decision

Based on the factors outlined above, the team has decided to enforce **Eventual Consistency** as a standard level of consistency across the board. This means that when a user performs a write operation, the system will not wait for the database to replicate the data immediately.

## Consequences

### Positive Consequences

* **High Availability**: Write operations will be highly available and responsive, ensuring that users can interact with the system without significant delays;
* **Low Cost**: This choice will lead to cost savings as it consumes fewer resources compared to stronger consistency levels. The project will benefit from a more cost-effective infrastructure;
* **Performance:** Write operations will be extremely fast, enhancing the user experience by reducing latency during data writes.

### Negative Consequences

* **Read Consistency Lag:** In some rare scenarios, users may infrequently experience small-to-negligible delays in read consistency, as the data on different replicas may not be immediately up to date;
* **Potential Stale Data:** Due to the less-strict consistency guarantees of Eventual Consistency, in scenarios of very heavy load combined with accessing databases of different regions, clients may (possibly, albeit extremely unlikely) end-up reading older data, but this is not a critical concern for the application as it doesn't require strong ordering guarantees.

## References
1. [Azure Cosmos DB Documentation - Consistency Levels](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
2. [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem)
3. [Consistency Architecture in Azure Cosmos DB](https://mostafaelmasry.com/2020/07/20/consistency-architecture-in-azure-cosmos-db/)
4. [Azure Cosmos DB Overview](https://vmayakumar.wordpress.com/2020/02/11/azure-cosmos-db-overview/)
