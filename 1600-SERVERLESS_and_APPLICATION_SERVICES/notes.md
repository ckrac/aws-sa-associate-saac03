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

### CloudWatch Events and EventBridge

- if **X** happens, or at **Y** time(s)... do **Z**
- EventBridge is... CloudWatch Events v2(\*)
- a **default** event bus for the account
- ...in CloudWatch Events this is the only bus (**implicit**)
- EventBridge can have additional event busses
- rules match incoming events... (or schedules)
- route the events to **1+ Targets**... eg. Lambda

EventBridge-1.png

### Serverless Architecture

_What is serverless_

- serverless **isn't one single thing**
- you manage **few**, **if any** servers - low overhead
- applications are a collection of small & specialized functions
- ...**stateless** and **ephemeral** environments - duration billing
- **event-driven**... consumption only when being used
- **FaaS** is used where possible for compute functionality
- **managed services** are used where possible

Serverless-1.png

### Simple Notification Service (SNS)

- **Public AWS Service** - network connectivity with Public Endpoint
- coordinates the sending and delivery of **messages**
- messages are <= **256KB** payloads
- **SNS Topics** are the base entity of SNS - **permissions** and **configuration**
- a **Publisher sends** messaged to a **TOPIC**
- **TOPICS** have **Subscribers** which **receive** messages
- e.g. HTTP(s), Email(-JSON), SQS, Mobile Push, SMS Messages & Lambda
- SNS used across AWS for notifications - e.g. CloudWatch & CloudFormation

SNS-1.png

- Delivery **Status** - (including HTTP, Lambda, SQS)
- Delivery **Retries** - Reliable Delivery
- **HA** and **Scalable** (Region)
- Server Side Encryption (**SSE**)
- Cross-Account via **TOPIC Policy**

### Step Functions

_Some problems with Lambda_

- Lambda is **FaaS**
- **15-minute max** execution time
- can be chained together
- gets messy at scale
- runtime environments are **stateless**

_State Machines_

- serverless workflow... **START** -> **STATES** -> **END**
- states are **THINGS** which occur
- maximum duration **1 year**
- **Standard** Workflow and **Express** Workflow
- started via API Gateway, IOT Rules, EventBridge, Lambda...
- Amazon States Language (**ASL**) - JSON Template
- **IAM Role** is used for permissions

_States_

- SUCCEED & FAIL
- WAIT
- CHOICE
- PARALLEL
- MAP
- TASK (Lambda, Batch, DynamicDB, ECS, SNS, SQS, Glue, SageMaker, EMR, Step Functions)

_Step Functions_

StepFunctions.png
