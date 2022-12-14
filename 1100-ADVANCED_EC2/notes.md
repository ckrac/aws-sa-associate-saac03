# ADVANCED EC2

### Bootstrapping EC2 using User Data

- bootstrapping allows **EC2 Build Automation**
- enabled using user data - accessed via the meta-data IP
- http://169.254.169.254/latest/user-data
- anything in User Data is **executed** by the **instance OS**
- executed **ONLY on Launch** (first initial launch)
- EC2 doesn't interpret, the OS needs to understand the User Data

EC2Bootstrapping-1.png

_User Data Key Points_

- it's **opaque** to EC2... its just a **block of data**
- it's **NOT** secure - don't use it for passwords or long term credentials (ideally)
- User Data is limited to 16 KB in size
- can be modified when instance stopped
- but **only executed once** at launch

How quick is it ready for service?

EC2Bootstrapping-2.png

### Enhanced Bootstrapping with CFN-INIT

- a way to pass complex bootstrapping instructions into an EC2 instance
- **cfn-init** helper script - installed on EC2 OS
- simple configuration management system
- procedural (user data) vs desired stater (cfn-init)
- can update packages, groups, users, sources, files, commands and services
- provided with directives via **Metadata** and **AWS::CloudFOrmation::Init** on a CFN resource

CFN-INIT-[1-2].png

### EC2 Instance Roles & Profile

EC2InstanceRoles-1.png

- credentials are inside meta-data
- iam/security-credentials/**role-name**
- automatically rotated - always valid
- should always be used rather than adding access keys into instance
- CLI tools will use ROLE credentials automatically

### SSM Parameter Store

- the SSM Parameter store is a service which is part of Systems Manager which allows the storage and retrieval of parameters - string, stringlist or secure string
- the service supports encryption which integrates with KMS, versioning and can be secured using IAM
- the service integrates with many AWS services and can be accessed using the CLI/APIs from anywhere with access to the AWS Public Spare Endpoints

- storage for **configuration** & **secrets**
- string, stringlist & secure string
- license codes, database strings, full configs & passwords
- hierarchies & versioning
- plaintext and ciphertext
- public parameters - **Latest AMIs per region**

SSMParameterStore.png

### System and Application Logging on EC2

_Logging on EC2_

- CloudWatch is for metrics
- CloudWatch Logs is for logging
- neither natively capture **data inside an instance**
- CloudWatch Agent is required... plus **configuration** and **permissions**

LoggingonEC2CWAgent.png

### EC2 Placement Groups

- three types of placement groups

PlacementGroups-1.png

- **Cluster** - Pack instances close together
  - offer little resistance because if hardware goes down, all ec2 instances goes down
  - can't span AZs - **ONE AZ ONLY** - locked when launching first instance
  - can span VPC peers - but impacts performance
  - requires a supported instance type
  - use the same type of instance (**not mandatory**)
  - launch at the same time (**not mandatory**, **very recommended**)
  - **10Gbps single stream performance**
  - **use cases: performance, fast speeds, low latency**

PlacementGroups-2.png

- **Spread** - Keep instances separated (using different hardware)
  - provides infrastructure isolation
  - ...each **INSTANCE** runs from a different rack
  - each rack has its own **network** and **power** source
  - **7 Instances per AZ** (HARD LIMIT)
  - not supported for dedicated instances or hosts
  - **use case**: small number of critical instances that need to be kept separated from each other (offers high resilience and availability)

PlacementGroups-3.png

- **Partition** - groups of instances spread apart (for distributed and replicated applications which have infrastructure awareness)
  - **7 Partitions per AZ**
  - instances can be placed in **a specific partition**
  - ...or auto placed
  - great for topology aware applications
  - ...HDFS, HBase, and Cassandra
  - contain the impact of failure to part of an application

### Dedicated Hosts

- EC2 Host **dedicated to you**
- specific family e.g. a1, c5, m5
- **no instance charges**... you pay for the host
- can pay on-demand & reserved options available
- host hardware has **physical sockets and cores**

  - dictates how many instances can be run on that host
  - software which is licensed based on physical sockets or cores can utilize this visibility of the hardware

  DedicatedHosts-[1-2].png

_Limitations & Features_

- **AMI Limits** - RHEL, SUSE Linux, and Windows AMIs aren't supported
- **Amazon RDS** instances are not supported
- **Placement groups** are not supported for Dedicated Hosts
- hosts can be shared with other ORG Accounts... **RAM** product

## Enhanced Networking & EBS Optimized

- Enhanced networking is the AWS implementation of SR-IOV, a standard allowing a physical host network card to present many logical devices which can be directly utilized by instances
- This means lower host CPU usage, better throughput, lower and consistent latency
- EBS optimisation on instances means dedicated bandwidth for storage networking - separate from data networking

_Enhanced Networking_

EC2EnhancedNetworking.png

_EBS Optimized_

- **EBS** = Block storage over the _network_
- historically network was **shared**... **data** and **EBS**
- EBS optimized means **dedicated capacity** for EBS
- most instances **support** and have **enabled by default**
- in older in instances some support, but enabling costs extra

## Implementing DNSSEC with R53

- DNSSEC strengthens authentication in DNS using digital signatures based on public key cryptography. With DNSSEC, it's not DNS queries and responses themselves that are cryptographically signed, but rather DNS data itself is signed by the owner of the data. This video steps though how DNSSEC is implemented in AWS Route53 and demos DNSSEC signing enabling on a hosted zone, including trust chain establishment.

DNSSEC.png
