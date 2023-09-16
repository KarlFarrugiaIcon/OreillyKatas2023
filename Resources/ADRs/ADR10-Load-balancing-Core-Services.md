# ADR 10: Load Balancing of Core Services

## Status  
Accepted

## Rationale
In previous [ADR 4](/Resources/ADRs/ADR04-Microservices-architecture.md) we have already identified that we will be having different services which will need to be hosted and orchestrated. It was proposed to use a Kubernetes Cluster to do this due to the greated control over the infrastructure or to use Azure Container Apps which is a managed service which abstracts away the complexities of using and managing low-level infrastructure.  

## Decision
It was decided to use a Kubernetes Cluster

## Consequences
Positive:
* Cheaper due to the management of infrastructure rather than using a managed service
* Cheaper due to the ability to of getting reserved instances

Negative:
* More complexity in managing the infrastructure