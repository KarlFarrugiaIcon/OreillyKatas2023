# ADR 9: Choice of "Eventual Consistency" for Azure Cosmos DB

## Status

Accepted

## Rationale

### Technological Considerations

When a user performs a write operation, we aim to provide a responsive and efficient experience. Relying on local storage to show the latest data aligns with our goal of minimizing latency for write operations. This approach allows us to prioritize the speed of write operations by not waiting for the database to replicate data immediately.

### Cost Implications

Our project places a strong emphasis on cost-effectiveness, and we want to optimize our resource utilization. Eventual consistency is the most cost-effective option among the five well-defined consistency levels offered by Azure Cosmos DB. It consumes fewer resources compared to stronger consistency levels, making it an ideal choice for our cost-conscious approach.

### Project Requirements

Our project's requirements favor high availability and partition tolerance, which are essential elements of the CAP theorem. We acknowledge that read consistency may lag behind the real data due to our choice of eventual consistency. However, this trade-off is acceptable in our context, as our primary focus is to ensure that write operations are fast and readily available to users.

## Decision

Based on the factors outlined above, we have decided to implement Azure Cosmos DB's **Eventual Consistency** level of consistency. This means that when a user performs a write operation, the system will not wait for the database to replicate the data immediately.

We will implement Eventual Consistency by allowing writes to proceed without waiting for immediate replication to all replicas. Users performing write operations may experience a temporary lag when reading data from different replicas, but this is an acceptable trade-off given our focus on high availability and cost-effectiveness.

## Consequences

### Positive Consequences

- **High Availability**: Write operations will be highly available and responsive, ensuring that users can interact with the system without significant delays.
- **Low Cost**: This choice will lead to cost savings as it consumes fewer resources compared to stronger consistency levels. Our project will benefit from a more cost-effective infrastructure.
- **Performance:** Write operations will be extremely fast, enhancing the user experience by reducing latency during data writes.

### Negative Consequences

- **Read Consistency Lag:** Users may experience a lag in read consistency, as the data on different replicas may not be immediately up to date. However, this is expected and acceptable for our application's use case.
- **Potential Stale Data:** Due to the weak consistency guarantees of Eventual Consistency, clients may occasionally read older data, but this is not a critical concern for our application as it doesn't require strong ordering guarantees.

## References
1. [Azure Cosmos DB Documentation - Consistency Levels](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
2. [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem)
3. [Consistency Architecture in Azure Cosmos DB](https://mostafaelmasry.com/2020/07/20/consistency-architecture-in-azure-cosmos-db/)
4. [Azure Cosmos DB Overview](https://vmayakumar.wordpress.com/2020/02/11/azure-cosmos-db-overview/)
