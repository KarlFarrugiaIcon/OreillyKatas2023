# ADR 10: Load Balancing of Core Services

## Status  
Approved

## Rationale
In a previous ADR ([ADR 4](/Resources/ADRs/ADR04-Microservices-architecture.md)), different services that need to be hosted and orchestrated have already been identified. A Kubernetes Cluster is proposed as the ideal way to do this due to the greater control over the infrastructure or to use managed services (such as Azure Container Apps) that abstract away the complexities of using and managing low-level infrastructure.  

## Decision
A Kubernetes Cluster or relevant managed service will be used.

## Consequences
Positive:
* Cheaper due to the management of infrastructure rather than using a managed service;
* Cheaper due to the ability to of getting reserved instances.

Negative:
* More complexity in managing the infrastructure.
