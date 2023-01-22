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

### API Gateway

_API Gateway - 101_

- service that **create** and **manage** APIs
- **endpoint**/entry-pint for applications
- sits **between applications** & **integrations** (services)
- highly **available**, **scalable**, handles **authorization**, **throttling**, **caching**, **CORS**, **transformations**, **OpenAPI** spec, **direct integration** and much more
- can connect to services/endpoints in **AWS** or **on-premises**
- **HTTP** APIs, **REST** APIs and **WebSocket** APIs

_Overview_

APIGateway-1.png

_Authentication_

APIGateway-2.png

_Endpoint Types_

- **edge-optimized** - routed to the nearest CloudFront POP
- **regional** - clients in the same region
- **private** - endpoint accessible only within a VPC via interface endpoint

_Stages_

APIGateway-3.png

_Errors_

- **4xx** - **Client Error** - Invalid request on **client** side
- **5xx** - **Server Error** - valid request, **backend** issue
- **400** - Bad Request - Generic
- **403** - Access Denied - Authorizer denies... WAF Filtered
- **429** - API Gateway can throttle - this means you've exceeded that amount
- **502** - Bad Gateway Exception - bad output returned by lambda
- **503** - Service Unavailable - backing endpoint offline? Major service issues
- **504** - Integration Failure/Timeout - 29s limit

_Caching_

APIGateway-4.png

### Simple Queue Service

- public, fully managed, highly-available queues - **Standard** or **FIFO**
- messages up to **256KB** in size - **link** to large data
- received messages are **hidden** (**VisibilityTimeout**)
- ...then either reappear (retry) or are explicitly deleted
- **dead-letter queues** can be used for problem messages
- ASGs can scale and Lambdas invoke based on queue length

SQS-1.png
SQS-2.png **Fanout Architecture**

- standard = **at-least-once**, FIFO (First-In-First-Out) = **exactly-once** and **guaranteed order**
- FIFO (performance) **3,000 messages per second** with batching, or up to **300 messages per second without**
- billed based on 'requests'
- 1 request = 1-10 messages up to 64KB total
- **short** (immediate) vs **long** (**waitTimeSeconds**) polling
- encryption at rest (**KMS**) & in-transit
- queue policy... access to queue, can allow to external accounts

### SQS Standard vs FIFO Queues

FIFOvsStandard.png

### SQS Delay Queues

- delay queues provide an initial period of invisibility for messages
- predefine periods can ensure that processing of messages doesn't begin until this period has expired

SQSDelayQueues.png

### SQS Dead-Letter Queues

- Dead letter queues allow for messages which are causing repeated processing errors to be moved into a dead letter queue
- in this queue, different processing methods, diagnostic methods or logging methods can be used to identity message faults

SQSDeadLetterQueues.png

### Kinesis Data Streams

_Kinesis Concepts_

- kinesis is a **scalable streaming** service
- producers _send_ data into a kinesis **stream**
- streams can scale from low to near infinite data rates
- public service & highly available by design
- streams store a **24-hour** moving window of data
- ...can be increased to a maximum of **365 days** (additional cost)
- multiple consumers access data from that moving window

_Architecture_

Kinesis.png

_SQS vs Kinesis_

- SQS **1** production group, **1** consumption group
  - **decoupling** and **asynchronous** communications
  - **no persistence** of messages, **no window**
- kinesis designed for **huge scale ingestion**
  - ...and **multiple consumers**... **rolling window**
  - data **ingestion**, **analytics**, **monitoring**, **app clicks**

### Kinesis Data Firehose

- fully managed service to load data for **data lakes**, **data stores** and **analytics services**
- **automatic scaling** ...fully **serverless** ...**resilient**
- **near real time** delivery (**60 seconds**)
- supports **transformation** of data on the fly (**lambda**)
- **billing** - **volume** through firehose

_Architecture_

KinesisFirehose-1.png

### Kinesis Data Analytics

- **real time processing** of data
- ...using structured query language (**SQL**)
- ingests from **Kinesis Data Streams** or **Firehose**
- destinations
- **Firehose** (S3, Redshift, ElasticSearch, Splunk)
- AWS **Lambda**
- Kinesis **Data Streams**

_Architecture_

KinesisDataAnalytics-1.png

_When and Where_

- streaming data needing **real-time SQL processing**
- time-series analytics ...**elections** / **e-sports**
- real-time dashboards - **leader boards** for games
- real-time metrics - **security** & **response** teams

### Kinesis Video Streams

- ingest **live video** data from **producers**
- **security cameras**, **smartphones**, **cars**, **drones**, **time-serialized**, **audio**, **thermal**, **depth**, and **RADAR** data
- consumers can access data **frame-by-frame** ... or as **needed**
- can **persist** and **encrypt** (in-transit and at rest) data...
- ...**can't access directly via storage ...only via APIs**
- integrates with other AWS services e.g. **Rekognition** and **Connect**

KinesisVideoStreams.png

### Amazon Cognito - User and Identity Pools

- cognito has terrible naming...
- **authentication**, **authorization**, and **user management** for web/mobile apps
- **USER POOLS** - **Sign-in** and get a JSON Web Token (**JWT**)
- User directory management and profiles, sign-up & sign-in (customizable web UI), MFA and other security features
- **IDENTITY POOLS** - Allow you to offer access to **Temporary AWS Credentials**
- **unauthenticated identities** - guest users
- **federated identities** - SWAP - Google, Facebook, Twitter, SAML2.0 & User Pool for short term AWS Credentials to access AWS Resources

_User Pools_

Cognito-1.png

_Identity Pools_

Cognito-2.png

_User & Identity Pools_

Cognito-3.png

### AWS Glue

- **serverless ETL** (**Extract**, **Transform** & **Load**)
- ...vs **datapipeline** (which can do ETL) and uses servers (**EMR**)
- **moves** and **transforms** data between **source** and **destination**
- crawls data sources and generates the AWS **Glue Data catalog**
- Data **Source**: **Stores**: S3, RDS, JDBC Compatible & Dynamo DB
- Data **Source**: **Streams**: Kinesis Data Stream & Apache Kafka
- Data **Targets**: S3, RDS, JDBC Databases

_Data Catalog_

- **persistent metadata** about data sources in region
- **one catalog** per **region** per **account**
- **avoids data silos**...
- Amazon Athena, Redshift Spectrum, EMR & AWS Lake Formation all use Data Catalog
- ...configure **crawlers** for data sources

Glue.png

### Amazon MQ 101

- **SNS** and **SQS** are **_AWS services_** - using **AWS APIs**
- SNS provides **TOPICS** and SQS provides **QUEUES**
- **public services**... **highly scalable**... **AWS Integrated**
- **many ORGS already use topics and queues**
- ...and want to **migrate into AWS**
- ...SNS and SQS won't work out of the box
- we need a **standards compliant** solution for **migration**

- an open-source message broker
- based on managed **Apache ActiveMQ**
- ...**JMS API** ...protocols such as **AMQP**, **MQTT**, **OpenWire** and **STOMP**
- provides **QUEUES** and **TOPICS**
- one-to-one or one-to-many
- **single instance** (Test, Dev, Cheap) or **HA Pair** (Active/Standby)
- VPC Based - **NOT A PUBLIC SERVICE** - **Private networking required**
- no AWS native integration ...delivers activeMQ product which you manage

_Architecture_

AmazonMQ.png

- **SNS** or **SQS** for most **new** implementations (**default**)
- **SNS** or **SQS** if **AWS integration is required** (logging, permissions, encryption, service integration)
- **Amazon MQ** If you need to **migrate from an existing system** with **little to no application change**encryption, service
- **Amazon MQ** If APIs such as **JMS** or protocols such as **AMQP**, **MQTT**, **OpenWIre**, and **STOMP** are needed
- **remember you need private networking for Amazon MQ**

### Amazon AppFlow

AmazonAppFlow-[1-2].png
