# ELASTIC COMPUTE CLOUD (EC2) BASICS

### Virtualization 101

- virtualization is the process of running more than one operating system on a piece of physical hardware or server

lesson link - http://www.brendangregg.com/blog/2017-11-29/aws-ec2-virtualization-2017.html

### EC2 Architecture

- EC2 Instances are **virtual machines** (OS+Resources)
- EC2 Instances run on **EC2 Hosts** (physical service hardware which AWS manages)
- **Shared** Hosts or **Dedicated** Hosts
- **Hosts - 1 AZ** - AZ Fails, Host Fails, Instances Fail

_What's EC2 Good for?_

- traditional **OS+Application** Compute
- **long-running** compute
- **server** style applications
- ...either **burst** or **steady-state** load
- **monolithic** application stacks
- **migrated** application workloads or **disaster recovery**

### EC2 Instance Type

- **Raw** CPU, Memory, Local Storage Capacity & Type
- **Resource Ratios**
- **Storage** and **Data** Network **Bandwidth**
- System Architecture / Vendor
- Additional Features and Capabilities

_Five Main Categories_

- **General Purpose** - Default - Diverse workloads, equal resource ratio
- **Compute Optimized** - Media Processing, HPC, Scientific Modelling, gaming, Machine Learning
- **Memory Optimized** - Processing large in-memory datasets, some database workloads
- **Accelerated Computing** - Hardware GPU, field programmable gate arrays (FGPAs)
- **Storage Optimized** - Sequential and Random IO - scale-out transactional databases, data warehousing, Elasticsearch, analytics workloads

Decoding EC2 Types - EC2InstanceTypes-1.png

https://ec2instances.info/
https://aws.amazon.com/ec2/instance-types/

General overview of type and when to use - EC2InstanceTypes-2.png

### EC2 SSH vs EC2 Instance Connect

connect via EC2 Connect per region when enabling certain inbound IPs for EC2 Instance https://ip-ranges.amazonaws.com/ip-ranges.json

### Storage Refresher

- **Direct** (local) attached Storage - Storage on the EC2 Host
  - problems - if disk or hardware fails, EC2 instance moves between hosts, the storage can be lost
- **Network** attached Storage - Volumes delivered over the network (EBS - Elastic Block Store)
  - generally high resilient and is separate from the Instance hardware
- **Ephemeral** Storage - Temporary Storage
- **Persistent** Storage - Permanent Storage - lives on past the lifetime of the instance
  - e.g. network attached storage by EBS

_Three categories of storage in AWS_

- how the storage is presented either to you or to a server and what it can be used for

StorageRefresher-1.png

_Storage Performance_

StorageRefresher-2.png

### Elastic Block Store (EBS)

- a service that provides block storage
- **Block** Storage - **raw** disk allocations (**volume**) - Can be encrypted using **KMS**
- ...instances see **block device** and create **file system** on this device (ext3/4, xfx)
- storage is provisioned in **ONE AZ** (Resilient **in that AZ**)
- attached to one EC2 Instance (or other service) over a storage network
- can be **detached** and **reattached**, not lifecycle linked to one instance... **persistent**
- **snapshot** (backup) into **S3**. Create volume from snapshot (migrate **between AZs**)
- can provision volumes based on different physical storage **types**, different **sizes**, different **performance profiles**
- billed based on **GB-month** (and in some cases **performance**)

### EBS Volume Types - General Purpose

EBSVolumes-[1-2].png

### EBS Volume Types - Provisioned IOPS

EBSVolumes-3.png

### EBS Volume Types - HDD-Based

EBSVolumes-4.png

### Instance Store Volumes - Architecture

- provide **Block Storage** Devices
- physically connected to **one EC2 Host**
- instances **on that host** can access them
- highest storage performance in AWS
- included in instance price...
- **ATTACHED AT LAUNCH**

InstanceStoreVolumes.png

- D3 = 4.6 GB/s throughput
- I3 = 16 GB/s of sequential throughput
- **More IOPS and Throughput vs EBS**

_Summary_

- local on **EC2 Host**
- add at **launch only**
- lost on instance **move**, **resize** or **hardware failure**
- high performance
- you pay for it anyway - included in instance price
- **TEMPORARY**

### Choosing between EC2 Instance Store and EBS

_important to remember figures_

InstanceStorevsEBS-1.png
InstanceStorevsEBS-2.png

### Snapshots, Restore & Fast Snapshot Restore (FSR)

- snapshots are incremental volume copies to **S3**
- the first is a **full copy** of 'data' on the volume
- future snaps are **incremental** (delta of data between snapshots)
- volumes can be created (restored) from snapshots
- snapshots can be copied to another region

_Architecture_
EBSSnapshots-1.png
EBSSnapshots-2.png

- new EBS volume - **full performance immediately**
- **snaps restore lazily** - fetched gradually
- requested blocks are fetched immediately
- force a read of all data immediately...
- Fast Snapshot Restore (**FSR**) - Immediate restore
  - ...up to **50** snaps per region. Set on the **Snap & AZ**

### EBS Encryption

EBSEncryption-[1-2].png

- accounts can be set to **encrypt by default** - **default KMS Key**
- otherwise **choose a KMS Key** to use
- each volume uses **1 unique DEK** (data encryption key)
- **snapshots** & **future volumes** use the **same DEK**
- can;t change a volume to **NOT** be encrypted
- OS isn't aware of the encryption ... **no performance loss**

### Network Interfaces, Instance IPs and DNS

EC2networking-[1-2].png

- secondary ENI + MAC = **Licensing**
- multi-homed (subnets) Management & Data
- different security groups - **multiple interfaces**
- OS - **Doesn't see public IPv4**
- IPv4 Public IPs are **Dynamic**... Stop & Start = **Change**
- public DNS = **private IP in VPC**, public IP everywhere else

### Amazon Machine Images (AMI)

- one way to create a template of an instance configuration and use that template to create many instances
- AMI's can be used to **launch EC2** instance
- **AWS** or **Community** Provided
- Marketplace (can include **commercial software**)
- **regional**... **unique ID** e.g. ami-0a887e401f7654935
- controls permissions (public, your account, specific accounts)
- you can create an AMI from an EC2 instance you want to template

AMILifecyle.png

- **AMI** = One Region, only works in that one region
- **AMI Baking**... creating an AMI from a configured instance + application
- an **AMI can't be edited**... launch instance, update configuration and make a new AMI
- can be copied **between regions** (includes its snapshots)
- remember permissions... **default = your account**

### EC2 Purchase Options (Launch Types)

EC2PurchaseOptions-[1-5].png

### Reserved Instances

EC2ReservedInstances-[1-3].png

### Instance Status Checks & Auto Recovery

InstanceStatusChecks.png

### Horizontal vs Vertical Scaling

EC2Scaling-[1-3].png

_Vertical Scaling_

- each resize requires a reboot - **disruption**
- larger instances often carry a **$ premium**
- there is an upper cap on performance - **instance size**
- **no application modification** required
- works for ALL applications - **even Monoliths**

_Horizontal Scaling_

- sessions, sessions, sessions (very important, session state is store in one instance. Different session when user interfaces with a different instance)
- requires application support OR **off-host sessions** (session/data store in db)
- **no disruption** when scaling
- **no limit** to scaling
- often less expensive - **no large instance premium**
- more granular

### Instance Metadata

EC2InstanceMetadata.png

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html
