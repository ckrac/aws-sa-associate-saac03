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
