# ADR 13: Usage of Serverless Functions with Redis Over APIs

## Status  
Conditionally accepted depending on scale

## Rationale
It was proposed to alleviate a serverless functions approach with Redis over the managing of REST APIs to make the solution more scalable and performant

## Decision
This was conditionally approved if the performance optimisation outweighed the cost. The usage of Redis was approved for either solution with a read through approach.

## Consequences
Positive:
* Better Performance
* More Flexibility and Scalability
* Easier to test

Negative:
* More costly