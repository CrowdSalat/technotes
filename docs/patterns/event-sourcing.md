# Notes on event sourcing

- Event Sourcing is a pattern in which there is not a single state (which gets updated), but a stream of events which can be folded to a state (for every given point in time)
- Event Sourcing is not CQRS
- **CQRS (Command-Query-Responsibility-Segregation)** is a database pattern that separates read and write models (you may have multiple read models). 
- Event sourcing is often used for the write model of a CQRS System. 
- A read model might be one that shows the current state (the folded events) and another one might show the state aggregated for every day.
- Command Query Segregation is a programming pattern which says that methods 
  - either have a return value but no side-effects 
  - or should create/update data (and no return value).
