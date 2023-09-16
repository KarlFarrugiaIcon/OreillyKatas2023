# ADR 11: Segregation of Core Services and Reader APIs

## Status  
Accepted

## Rationale
Given that the solution can be split over system-based events and user-driven events there was a proposal to split the core services and reader APIs into different hosting. This was proposed with the intention of protecting core services from being exposed to the public and to have them load balance outside of seasonal or erratic traffic.

## Decision
It was agreed to have the core services and reader APIs split.

## Consequences
Positive:
* Better Separation of Concerns
* Better Performance

Negative:
* More complex orchestration