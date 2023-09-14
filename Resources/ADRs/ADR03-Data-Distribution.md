
## Cosmos DB and Redis for Global Data Distribution

## Rationale 
In our architectural discussions, we addressed the need for global data distribution to ensure low-latency access to data for users in different geographic regions. We evaluated several options and have made the decision to use Cosmos DB in combination with Redis for this purpose

## Decision 
After careful consideration of our project's requirements and objectives, we have chosen to implement a solution that involves using Cosmos DB and Redis to achieve global data distribution. This approach ensures data is efficiently disseminated from the primary Cosmos DB to regional read/write Cosmos DB instances, which are further responsible for dispersing information to other Cosmos DB instances within their respective regions.

## Consequences
Positive:
+ Cosmos DB ensures low-latency access for users worldwide 
+ Cosmos DB can scale horizontally to handle high-throughput workloads
+ Redis serves as an efficient in-memory cache, reducing the load on the primary Cosmos DB by storing frequently accessed data,

Negative:  
+ Redis may cause synchronization issues
+ Operating and maintaining both Cosmos DB and Redis instances, along with monitoring and ensuring data consistency, requires careful planning and resources
