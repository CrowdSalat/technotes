
# event driven architecture 

## saga pattern

## orchestration vs. choreography

## links 

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
    - 