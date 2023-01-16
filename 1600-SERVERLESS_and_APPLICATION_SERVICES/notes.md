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

### AWS Lambda

- function-as-a-service (**FaaS**) - **short running** & **focused**
- lambda function - a **piece of code** lambda runs
- functions use a **runtime** (e.g. Python 3.8)
- functions are loaded and run in a **runtime environment**
- the environment has a **direct memory** (**indirect CPU**) allocation
- you are billed for the **duration that a function runs**
- a key part of **Serverless** architectures

Lambda-1.png

_Common Uses_

- **serverless** applications (**S3**, **API** Gateway, **Lambda**)
- file processing (**S3**, S3 **Events**, **Lambda**)
- database triggers (**DynamoDB**, **Streams**, **Lambda**)
- serverless CRON (**EventBridge**/**CWEvents** + **Lambda**)
- realtime stream data processing (**Kinesis** + **Lambda**)

_Public Lambda Networking_

Lambda-2.png

_Private Lambda Networking_

Lambda-[3-4].png

_Lambda Invocation_

- **synchronous** invocation
- **asynchronous** invocation
- **event source mappings**

Lambda-[5-7].png

_Lambda Versions_

- lambda functions have **versions** - **v1**, **v2**, **v3**...
- a version is the **code** + the **configuration** of the lambda function
- it's **immutable** - it never changes once published & has its own **Amazon Resource Name**
- **$Latest** points at the latest version
- **aliases** (DEV, STAGE, PROD) point at a version - **can be changed**

_Lambda Startup Times_

Lambda-8.png
