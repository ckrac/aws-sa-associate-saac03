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
