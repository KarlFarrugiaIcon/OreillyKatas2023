## Finalists: Autumn/Fall 2022 

> GitHub Repositories of the top 3 and runners-up of the O'Reilly Autumn/Fall Architecture Kata 2022.

1. [monArch](https://github.com/mynksnh/ArchKatas2022)
2. [IPT](https://github.com/iptch-public/2022-fall-architectural-katas)
3. [The Black Cat Manifestation](https://github.com/jjones203/architecture_kata)

### Runners-Up

> Listed in alphabetical order with no ranking.

- [Achievers](https://github.com/The-Achievers-Team/ArchKatas2022)
- [ItDepends](https://github.com/ITDependsQ42022/Hey-Blue)
- [Los Ingenials](https://github.com/ingeniala/heybluekata)

# DOs

1. Use ADRs for justifying architectural decisions
2. Diagrams
   1. Colours
   2. Easy to read
   3. No huge diagrams
   4. Different levels of diagrams
3. Narrative (go over certain steps of the design as if it was a story)
4. Properly explain each diagram ([Good example](https://github.com/mynksnh/ArchKatas2022); [bad example](https://github.com/ITDependsQ42022/Hey-Blue))

# DONTS

1. Use the same colour for all diagrams
2. Properly explain each diagram
3. Lots of README links jumping from one place to another

# APPROACHES

## monArch

1. Context
2. [Determine the architectural characteristics](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#architecture-characteristics)
3. [Determined the architecture](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#architecture-approach)
   1. The C4 model was used
   2. Microservices were used even though it scored lower than event-driven. This was justified with a reason
4. [Identified business context](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#context)
   1. Actors and use cases
   2. Event storming
      1. Started with domain events
      2. Added triggers
      3. Automation policies and bounded contexts
5. [Containerising](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#containers)
   1. Identified the bounded context with a modular monolith
   2. Segregate the bounded contexts into services
   3. Included an API layer for communication
   4. Discussion on coupling of the services
6. [Identifying components](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#components)
   1. Description on each of the components identified in the previous step
   2. Description of some workflows
7. [Proposal of the deployment procedure](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#deployment)
8. [Basic cost analysis](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#cost-analysis)
9. [Self evaluation on the architecture chosen and small discussion on potential risks](https://github.com/mynksnh/ArchKatas2022/blob/main/Readme.md#evaluation-risks-and-architecture-fitness)

## IPT

1. Requirements
2. [Actor overview](https://github.com/iptch/2022-fall-architectural-katas#actors-overview)
3. [Business context](https://github.com/iptch/2022-fall-architectural-katas#context)
4. [Designed the domain](https://github.com/iptch/2022-fall-architectural-katas#domain-design)
   1. [Event storming](https://github.com/iptch/2022-fall-architectural-katas#event-storming-process)
   2. [Choice of architecture](https://github.com/iptch/2022-fall-architectural-katas#architecture-style)
   3. [Splitting the events into services](https://github.com/iptch/2022-fall-architectural-katas#domain-capabilities)
   4. [Detailed description of each service](https://github.com/iptch/2022-fall-architectural-katas#capabilities-in-depth)
5. [Overview of system architecture](https://github.com/iptch/2022-fall-architectural-katas#system-architecture)

# TECHNOLOGY

Output was generally kept technology-agnostic.


# COMMON OUTPUTS

- Everything in a single README (2 of the finalists and 1 of the semi-finalist did this)
- Content is split across multiple files (1 of the finalists and 2 of the semi-finalist did this)

# TOOLS USED

Generally not specified

# USEFUL REFERENCES

- [C4 Model](https://c4model.com/)  
- [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/)  
- [Fundamentals of Software Architecture](https://learning.oreilly.com/library/view/fundamentals-of-software/9781492043447/)  
- [Building Microservices](https://learning.oreilly.com/library/view/building-microservices-2nd/9781492034018/)  
- [Building Evolutionary Architectures](https://learning.oreilly.com/library/view/building-evolutionary-architectures/9781491986356/)  
- [Building Event-Driven Microservices](https://learning.oreilly.com/library/view/building-event-driven-microservices/9781492057888/)  
- [Architecture: The Hard Parts](https://alistair.cockburn.us/hexagonal-architecture/)  
- [Documenting Software Architectures Views and Beyond](https://learning.oreilly.com/library/view/documenting-software-architectures/9780132488617/)  
- [Previous Kata Entries](https://github.com/tekiegirl/SoftwareArchitectureResources/blob/main/Resources/OReillyKata.md)
