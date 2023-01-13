# SERVERLESS AND APPLICATION SERVICES

### Architecture Deep Dive

_Evolution from monolith -> tiered -> event-driven architecture_

Architecture-[1-6].png

_Event-Driven Architecture_

- **no constant running** or **waiting** for things
- **producers** generate events when something happens
  - ...clicks, errors, criteria met, uploads, actions
- events are delivered to **consumers**
  - ...**actions are taken** & the system returns to waiting
- mature event-driven architecture **only consumes resources while handling events** (serverless... more on this soon)
