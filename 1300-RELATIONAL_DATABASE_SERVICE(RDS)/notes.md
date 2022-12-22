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