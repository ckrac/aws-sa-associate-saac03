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
