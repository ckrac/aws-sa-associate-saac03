# VIRTUAL PRIVATE CLOUD (VPC) BASICS

### VPC Sizing and Structure

_VPC Considerations_

- what **size** should the VPC be?
- are there any Networks **we can't use**?
- mindful of VPC's, Cloud, On-premises, Partners & Vendors
- try to predict the **future**...
- VPC **Structure** - Tiers & Resiliency (Availability) Zones

![Global Architecture](../00_LEARNINGAIDS/VPCConsiderations-1.png)
![Global Architecture](../00_LEARNINGAIDS/VPCConsiderations-2.png)

- VPC minimum **/28** (16 IP), maximum **/16** (65536 IPs)
- personal preference for the **10.x.y.z** range
- **avoid common ranges** - avoid future issues e.g. 10.1 - 10.10, 10.128 - 10.255 (used by google)
- reserve 2+ networks per region being used per account
  - e.g. 3 US, Europe, Australia - Assume 4 accounts (total-regions x 2 \* number-of-accounts) = 40 ranges (ideally)
  - 10.16 - 10.127

_VPC Proposal_

- Animals4life could become a **huge** global entity
- Use the **10.16 -> 10.127** range (avoiding google)
- start a 10.16 (US1), 10.32 (US2), 10.48 (US3), 10.64 (EU), 10.80 (Aus) - each AWS account has 1/4th
- **/16** per VPC - 3 AZ (+1), 3 Tiers (+1) - **16** subnets
- **/16** split into 16 subnets = **/20** per subnet (4091 IPs)

### Custom VPCs

- regional service - All AZs in the region
- isolated network
- nothing **IN** or **OUT** without explicit configuration
- flexible configuration - simple or multi-tier
- supports hybrid networking - other cloud & on-premises
- option to pick **default** or **dedicated tenancy!**
  - controls whether the resources created inside the VPC are provisioned on shared hardware or dedicated hardware
- can use IPv4 Private CIDR Blocks & Public IPs
- a VPC is allocated 1 primary private IPv4 CIDR Block
  - has two main restrictions... Min **/28** (16 IP) Max **/16** (65,536)
- optional secondary IPv4 Blocks
- can be configured to use single **assigned** IPv6 **/56** CIDR Block

_DNS in a VPC_

- provided by R53
- VPC `Base IP +2` Address e.g. if VPC is 10.0.0.0, then the DNS IP will be 10.0.0.2
- **enableDnsHostnames** - gives instances DNS Names (indicates whether instances with public IP addresses in a VPC are given public DNS host names)
- **enableDnsSupport** - enables DNS resolution in VPC (indicates if DNS is enabled or enabled)

### VPC Subnets

- subnets are what services run from inside VPCs and they're how you add structure, functionality, and resilience to VPCs
- **AZ Resilient**
- a **subnetwork** of a VPC - **within a particular AZ**
- 1 Subnet => 1 AZ, 1 AZ => 0 + Subnets (subnet can never be in multiple AZ)
- IPv4 CIDR is a subset of the VPC CIDR
- cannot overlap with other subnets
- optional IPv6 CIDR (/64 subset of the /56 VPC - space for 256)
- subnets can communicate with other subnets in the VPC

_Subnet IP Addressing_

- Reserved IP addresses in a subnet (5 in total)
- eg. 10.16.16.0/20 (10.16.16.0 => 10.16.31.255)

1. **network** address (10.16.16.0)
2. **network +1** (10.16.16.1) - used by VPC Router
3. **network +2** (10.16.16.2) - Reserved (DNS\*)
4. **network +3** (10.16.16.3) - Reserved Future Use
5. **broadcast** address 10.16.31.255 (Last IP in subnet)

### VPC Routing and Internet Gateway

- every VPC has a VPC Router - Highly available
- in every subnet... `network+1` address
- routes traffic between subnets
- controlled by `route tables` each subnet has one
- a VPC has a **Main** route table - subnet default

_Internet Gateway (IGW)_

- **region resilient** gateway attached to a VPC
- 1 to 1 relationship, 1 VPC = 0 or 1 IGW, 1 IGW = 0 or 1 VPC
- runs from within the AWS Public Zone
- gateways traffic between the VPC and the Internet or AWS Public Zone (S3...SQS...SNS...etc)
- managed - AWS handles performance

_Bastion Host / Jumpbox_

- bastion host = jumpbox
- an instance in a public subnet
- incoming management connections arrive there
- then access internal VPC resources
- often the only way IN to a VPC

### Stateful vs Stateless Firewalls

StatefulvsStatless 00_LEARNINGAIDS

### Network Access Control Lists (NACL)

- can be thought of as a traditional firewall available within AWS VPCs
- **stateless** - **REQUEST** and **RESPONSE** seen as different
- only impacts **data crossing subnet boundary**
- NACLs can **EXPLICITLY ALLOW** and **DENY**
- IPs/CIDR, Ports & protocols - no logical resources
- NACLs cannot be assigned to AWS resources - **only subnets**
- use together with Security Groups to add explicit DENY (Bad IPs/Nets)
- each subnet can have **one NACL** (Default or Custom)
- a NACL can be associated with **MANY Subnets**

### Security Groups

- **STATEFUL** - **detect response** traffic **automatically**
- **Allowed** (IN or OUT) **request** = **allowed response**
- **NO EXPLICIT DENY**... only ALLOW or Implicit DENY
- ...**can't block specific bad actors**
- supports IP/CIDR and **logical resources**
- ...including other **security groups** AND ITSELF
- Attached to ENI's (elastic network interface) not instances (even if the UI shows it this way)

### Network Address Translation (NAT) & NAT Gateway

_What is NAT?_

- Network Address Translation (**NAT**)
- a set of processes - remapping SRC or DST IPs
- **IP masquerading** - hiding CIDR Blocks behind on IP
  - popular because IPv4 Addresses running out
- gives Private CIDR range **outgoing** internet\* access

_NAT Gateways_

- runs from a **public subnet**
- uses **Elastic IPs** (Static IPv4 Public)
- _AZ resilient Service_ (HA in that AZ)
- for region resilience - **NATGW in each AZ**... RT in for each AZ with that NATGW as target
- managed, scales to 45 Gbps, $ Duration & Data Volume

_What about IPv6_

- NAT isn't required for IPv6
- all IPv6 addresses in AWS are publicly routable
- the Internet Gateway works with ALL IPv6 IPs directly
- NAT Gateways **don't work with IPv6**
- ::/0 Route + IGW for bi-directional connectivity
- ::/0 Route + Egress-Only Internet Gateway - Outbound Only

### Gateway Load Balancer

_Why do we need a GWLB?_

GWLB-1.png

_GWLB... what is it?_

- help you **run** and **scale** **3rd party appliances**
- ...things like **firewalls**, **intrusion detection** and **prevention** systems
- **inbound** and **outbound** traffic (**transparent** inspection and protection)
- ...**GWLB endpoints**... traffic enters/leaves via these endpoints
- ...the GWLB **balances** across **multiple backend appliances**
- traffic and metadata is tunnelled using **GENEVE** protocol

_GWLB 101 - How it works?_

GWLB-2.png

GWLB-3.png
