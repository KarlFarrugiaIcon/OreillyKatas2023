# ADR 12: Distributing Data Globally

## Status  
Accepted

## Rationale
Based of previous ADRs were it was chosen to use CosmosDB [(ADR 9)](/Resources/ADRs/ADR09-CosmosDB-Consistency.md) and to segregate core services and reader apis [(ADR 10)](/Resources/ADRs/ADR11-Segregation-of-Core-Services-and-Reader-APIs.md) it was further proposed to host the reader APIs closer to the user's by provisioning them in different geographical regions and to have CosmosDB geographically replicated to have the data closer to the user.

## Decision
This was approved

## Consequences
Positive:
* Better Performance

Negative:
* More complex maintenance 