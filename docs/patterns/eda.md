
# event driven architecture 

## saga pattern

## orchestration vs. choreography

## event sourcing

- Event Sourcing is a pattern in which there is not a single state (which gets updated), but a stream of events which can be folded to a state (for every given point in time)
- Event Sourcing is not CQRS
- **CQRS (Command-Query-Responsibility-Segregation)** is a database pattern that separates read and write models (you may have multiple read models). 
- Event sourcing is often used for the write model of a CQRS System. 
- A read model might be one that shows the current state (the folded events) and another one might show the state aggregated for every day.
- Command Query Segregation is a programming pattern which says that methods 
  - either have a return value but no side-effects 
  - or should create/update data (and no return value).

## links 

- [small bites, nicely visualized](https://serverlessland.com/event-driven-architecture/visuals)
- [Martin Fowler](https://martinfowler.com/articles/201701-event-driven.html)
- [Martin Fowler GOTO;](https://www.youtube.com/watch?v=STKCRSUsyP0)
- [Confluent](https://martinfowler.com/articles/201701-event-driven.html)

- Event notification: Just a notification that something (some Data with the ID) changed. More information need to be queued in the source system
  - Pro: 
    - decoupling
    - the source for the notification (e.g. change) is admitted as 'business data' itself
  - Con: 
    - hard to find out whats going on in the whole system
- Event-carried state transfer: Enough information that the downstream system do not need to ask more questions (call the services of the source system)
  - Pro: 
    - decoupling
    - reduce load to source system
  - Con: 
    - Eventual consistency (replicated data)
- Event Sourcing: you keep a log of changes which can be used to create the state (e.g. accounting ledgers, source code version control)
  - Pro:
    - Alternative state
    - Auditing
    - 
  - Con:
    - 



