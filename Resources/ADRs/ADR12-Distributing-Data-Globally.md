# ADR 12: Distributing Data Globally

## Status  
Approved

## Rationale
Based on points raised in another ADRs, the usage of Distributed Databases was proposed (through managed services such as CosmosDB) [(ADR 9)](/Resources/ADRs/ADR09-CosmosDB-Consistency.md); and in another ADR, to segregate core services and reader apis [(ADR 10)](/Resources/ADRs/ADR11-Segregation-of-Core-Services-and-Reader-APIs.md) it was further proposed to host the reader APIs closer to the users by provisioning them in different geographical regions and to have CosmosDB geographically replicated to have the data closer to the user.

## Decision
This approach will be followed.

## Consequences
Positive:
* Better Performance.

Negative:
* More complex maintenance.
