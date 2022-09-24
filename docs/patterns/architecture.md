# architecture stuff

## KPIs for software delivery (four key metrics)

[Source: State of devops](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf)

- deployment frequency (speed) - 
  - Number of deployment to production
  - Measure with pipeline job history
- lead time for change (speed) 
  - Time from commit to production
  - Measure with git history: first commit - commit in main branch
- mean time to restore (stability) 
  - Time until a failure is fixed
  - Measure: Log protocols, probably manually
- change failure rate (stability) 
  - release/failure
  - Measure with reverts or bugfix commit in pipeline job history  


Metrics:
  - min
  - max
  - mean


## evolutionary apis

- consumer drive api - e.g. marketplaces like atlassian plugins define an api you must implement
  
- Zalando Style guides for APIs
- no versioning in urls because then clients need to adopt
- robustness principle / postals law (Be conservative in what you do, be liberal what you accept from other)
- parallel change pattern (expand, ) - to avoid versioning

- Consumer driven contracts 
  - contract testing tool: 
    - PACT (code first)
    - spring cloud contract first

- 
- bypass-API - backend for frontend