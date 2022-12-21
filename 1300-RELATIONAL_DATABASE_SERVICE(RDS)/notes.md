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
