# Relational Database Service (RDS)

### Database Refresher & Models

_Relational (SQL) vs None-Relational (NoSQL)_

- Structured Query Language (**SQL**)
- structure **in** & **between** tables of data - **Rigid Schema**
- ...relationships between tables
- NoSQL - **not one single thing... different models**
- generally a much more relaxed schema
- relationships handled differently

DBRefresher-[1-6].png

### ACID vs BASE

- **ACID** and **BASE** are DB **transaction models**
- **CAP Theorem** - **Consistency**, **Availability**, **Partition Tolerant** (resilience) - **Choose 2** (DB is capable of delivering only two)
- **ACID** = consistency
- **BASE** = availability

ACIDvBASE-[1-2].png

### Databases on EC2

DBonEC2-1.png

- generally bad practice

_Why might you do it..._

- access to the DB Instance **OS**
- **advanced DB Option tuning**... (DBROOT)
- ...vendor demands...
- **DB or DB Version AWS don't provide**...
- specific **OS/DB Combination** AWS don't provide
- architecture AWS don't provide (replication/resilience)
- decision makers who '**just want it**'

_Why you shouldn't really..._

- **admin overhead** - managing EC2 and DBHost
- **backup** / DR Management
- EC2 is **single AZ**
- **features** - some of AWS DB products are amazing
- EC2 is **ON** or **OFF** - no serverless, no easy scaling
- **replication** - skills, setup time, monitoring & effectiveness
- **performance** - AWS invest time into optimization & features

### Relational Database Service (RDS) Architecture

- Relational Database Service (RDS) is a Database(server) as a service product from AWS which allows the creation of managed databases instances
- not what RDS - Database as a Service (**DBaaS**)
- what it is - Database Server as a Service (**DBSaaS**)
- ...multiple databases on one DB Server (instance)
- choice of DB Engines (**MYSQL**, **MariaDB**, **PostgreSQL**, **Oracle**, Microsoft **SQL Server**)
- ...Amazon Aurora is a different product
- managed service... **no access** to **OS** or **SSH** Access

RDSArch-[1].png

_Costs_

- cost #1 - **instance size** & **type**
- cost #2 - **multi AZ** or not
- cost #3 - **storage type** & **amount**
- cost #4 - **data** transferred
- cost #5 - backups & snapshots
- cost #6 - licensing (if applicable)

### Relational Database Service (RDS) MultiAZ - Instance and Cluster

- MultiAZ is a feature of RDS which provisions a highly available instance set.
- the product provides MultiAZ instance .. where a standby replica is kept in sync Synchronously with the primary instance.The standby replica cannot be used for any performance scaling ... only availability.
- it also provides MultiAZ cluster mode, where a write and two reader instances are kept in sync Synchronously. The reader instances can be used for read operations ..allowing for limited read scaling.
- backups, software updates and restarts can take advantage of MultiAZ to reduce user disruption.

RDSMultiAZInstance.png

- Data => Primary **AND** Replicated to StandBy = **Committed** (**Synchronous**)
- **not free tier**... extra cost for replica
- **ONE** StandBy replica **ONLY**
- ...which **can't be used** for reads or writes
- job is for failover events, **60-120** seconds failover
- **same region only**... different AZs in the same region
- **backups** taken from **standby** to **improve performance**
- AZ outage, Primary Failure, Manual failover, Instance type change and software patching

RDSMultiAZCluster.png

- **1 Writer** and **2 Reader** DB instances (different AZs)
- ...much faster hardware, graviton + local **NVME SSD Storage**
- ...fast writes to **local storage** => **flushed to EBS**
- ...**readers** can be used for **reads**... allowing some **read scaling**
- replication is done via transactions logs... more efficient
- failover is faster - **35s** + **transaction log apply**
- writes are "**committed**" when **1 reader** has confirmed

### RDS Automatic Backup, RDS Snapshots and Restore

- RDS is capable of performing Manual Snapshots and Automatic backups
- manual snapshots are performed manually and live past the termination of an RDS instance
- automatic backups can be taken of an RDS instance with a 0 (Disabled) to 35 Day retention
- automatic backups also use S3 for storing transaction logs every 5 minutes - allowing for point in time recovery
- snapshots can be restored .. but create a new RDS instance

_Backups - Cross-Region_

- RDS can **replicate** backups to **another region**
- ...both **snapshots** and **transaction logs**
- charges apply for the **cross-region data copy**
- ...and the **storage** in the **destination region**
- **NOT DEFAULT**... configured within automated backups

_Restores_

- creates a **NEW RDS Instance** - **new address**
- snapshots = **single point in time**, creation time
- automated = **any 5 minute point in time**
- backup is restored and transaction logs are 'replayed' to bring DB to desired point in time (**GOOD RPO (Recovery Point Objective)**)
- restores **aren't fast** - think about **RTO (Recovery Time Objective)** (**RR's**)

### RDS Read Replicas

- RDS Read Replicas can be added to an RDS Instance - **5** direct per primary instance.
- they can be in the same region, or cross-region replicas
- they **provide read performance scaling** for the instance, but also offer low RTO recovery for any instance failure issues
- N.B they don't help with data corruption as the corruption will be replicated to the RR
- read-replicas can gave read-replicas - **but lag starts to be a problem**
- **global** performance improvements

_RPO (Recovery POint Objective)/RTO (Recovery Time Objective) Improvements_

- snapshots & backups improve **RPO**
- **RTO's are a problem**
- RR's offer **nr. 0 RPO** (near zero RPO)
- RR's can be **promoted quickly** - **low RTO**
- **failure only** - watch for data corruption
- **read only - until promoted**
- **global availability improvements... global resilience**

### Amazon RDS Security

- **SSL**/**TLS** (**in transit**) is available for RDS, can be **mandatory**
- RDS supports **EBS volume** encryption - **KMS**
- handled by **HOST**/**EBS**
- AWS or customer managed CMK generates **data keys**
- **data keys** used for **encryption operations**
- **storage**, **logs**, **snapshots** & **replicas** are encrypted
- ...**encryption can't be removed**
- RDS **MSSQL** and RDS **Oracle** Support **TDE (Transparent Data Encryption)**
  - encryption handled **within the DB engine**
- RDS **Oracle** supports integration with **CloudHSM**
- much stronger key controls (even from AWS)

RDSSecurity-[1-2].png

### RDS (Relational Database Service) Custom

- **fills the gap** between **RDS** and **EC2** running a DB Engine
- **RDS** is **fully managed** - **OS/Engine access is limited**
- DB on EC2 is self managed - but has overhead
- currently works for **MS SQL** and **Oracle**
- can connect using **SSH**, **RDP**, **Session Manager**
- RDS Custom **Database Automation**
- ...**pause** to customize (so there are no disruptions)
- ...resume (**full automation**) for normal production usage

### Aurora Architecture

- aurora is a AWS designed database engine officially part of RDS
- aurora implements a number of radical design changes which offer significant performance and feature improvements over other RDS database engines
- aurora architecture is **VERY** different from RDS...
- ...uses a "**Cluster**"
- a single **primary** instance + **0** or more **replicas**
- no local storage - uses **cluster volume**
  - faster provisioning & improved availability & performance

AuroraArch-1.png

- all SSD Based - **high IOPS**, **low latency**
- storage is billed based on **what's used**
- **high water mark** - billed for the most used
- storage which is freed up can be re-used
- replicas can be added and removed without requiring storage provisioning

AuroraArch-2.png

_Cost_

- no free-tier option
- aurora doesn't support Micro Instances
- beyond RDS singleAZ (micro) Aurora offers better value
- compute - hourly charge, per second, 10 minute minimum
- storage - GB-Month consumed, IO cost per request
- 100% DB Size in backups are included

- backups in Aurora work in the same way as RDS
- restores create a **new cluster**
- backtrack can be used which allow **in-place rewinds** to a previous point in time
- fast clones make a new database MUCH faster than copying all the data - **copy-on-write**

### Aurora Serverless

- scalable - **ACU** - Aurora Capacity Units
- Aurora Serverless cluster has a **MIN & MAX ACU**
- cluster adjusts based on load
- can go to _0_ and be **paused**
- consumption billing per-second basis
- same resilience as Aurora (6 copies across AZs)

AuroraServerless-1.png

_Use Cases_

- **infrequently** used applications
- **new** applications
- **variable** workloads
- **unpredictable** workloads
- **development** and **test** databases
- **multi-tenant** applications

### Aurora Global Database

- aurora global databases are a feature of Aurora Provisioned clusters which allow data to be replicated globally providing significant RPO and RTO improvements for BC and DR planning
- additionally global databases can provide performance improvements for customers
- ...with data being located closer to them, in a read-only form
- great for **cross-region DR (disaster recovery) and BC (business continuity)**
- **global read scaling - low latency performance improvements**
- replication occurs at the storage layer and is generally **~1second or less** between all AWS regions
- **no impact** on DB performance
- secondary regions can have **16 replicas**
- ...can be promoted to R/W
- currently MAX 5 secondary regions...

AuroraGlobalDB.png

### Multi-master writes

- Multi-master write is a mode of Aurora Provisioned Clusters which allows multiple instances to perform reads and writes at the same time - rather than only one primary instance having write capability in a single-master cluster
- default Aurora mode is **Single-Master**
  - **1 R/W **and **0+ Read Only** replicas
- cluster endpoint is used to write, read endpoint is used for load balanced reads
- failover takes time - replica promoted to R/W
- in multi-master mode **all instances are R/W**

AuroraMultiMaster-[1-3].png

### Relational Database Service (RDS) - RDS Proxy

_Why?_

- **opening** and **closing** connections **consume resources**
- ...it takes **time**... which **creates latency**
- with serverless... **every lambda** opens and closes?
- handling **failure** of Database instances is **hard**...
- ...doing it within your application **adds risks**
- DB **Proxies** help... **managing** them is not trivial (**scaling**/**resilience**)
- application(s) => Proxy (connection pooling) => Database

_Architecture_

RDSProxy.png

_When?_

- **too many connections** errors...
- DB instances using T2/T3 (i.e. **smaller**/**burst**) **instances**
- **AWS Lambda**... **time saved**/connection reuse & **IAM Auth**
- long running connections (SAAS apps) - low latency
- where **resilience** to database failure is a priority
- ...RDS proxy can **reduce the time for failover**
- ...and make it **transparent to the application**

_Key facts_

- **fully managed** DB **Proxy** for **RDS**/**Aurora**
- ...**auto scaling**, **highly available** by default
- provides **connection pooling** - reduces DB Load
- ONLY **accessible** from **a VPC**
- accessed via **Proxy Endpoint** - no app changes
- can enforce **SSL**/**TLS**
- can **reduce failover time** by **over 60%**
- **abstracts failure** away from your applications

### Database Migration Service (DMS)

- Database Migration Service (DMS) is a managed service
- which allows for 0 data loss, low or 0 downtime migrations between 2 database endpoints
- runs using a **replication instance**
- **source** and **destination endpoints** point at...
- **source** and **target** databases
- **one endpoint MUST be on AWS**

Database Migration Service.png

_Schema Conversion Tool (SCT)_

- SCT is used when converting **one database** engine to **another**
- ...including **DB** -> **S3** (Migrations using DMS)
- SCT is **not used when migrating between DB's of the same type**
- ..on-premises MySQL -> RDS MySQL
- works with **OLTP** DB Types (MySQL, MSSQL, Oracly)
- works with **OLAP** DB Types (Teratata, Oracle, Vertica, Greenplum...)
- e.g. on-premises **MSSQL** -> **RDS MySQL**
- e.g. on-premises **ORACLE** -> **Aurora**

_(DMS) & Snowball_

- larger migrations might be multi-TB in size
- ...moving data over networks takes time and consumes capcacity
- DMS can utilize snowball...
- **Step 1**: Use SCT to extract data locally and move to a snowball device
- **Step 2**: Ship the device back to AWS. They load onto an S3 bucket
- **Step 3**: DMS migrates from S3 into the target store
- **Step 4**: Change Data Capture (CDC) can capture changes, and via S3 intermediary they are also written to the target database
