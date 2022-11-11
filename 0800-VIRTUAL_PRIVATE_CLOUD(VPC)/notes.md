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
