# Route 53 - Global DNS

### R53 Public Hosted Zones

- A public hosted zone is a container that holds information about how you want to route traffic on the internet for a specific domain which is accessible from the public internet
- a **R53 Hosted Zone** is a DNS DB for a domain e.g. animals4life.org
- **globally resilient** (multiple DNS Servers)
- created with domain registration via R53 - can be created separately
- Host **DNS Records** (e.g. A, AAAAA, MX, NS, TXT...)
- Hosted Zones are what the DNS system references - **Authoritative** for a domain e.g. Animals4life.org
- DNS Database (**zone file**) hosted by R53 (**Public Name Servers**)
- accessible from the **public internet** & **VPCs**
- hosted on "**4**" R53 Name servers (NS) specific for the zone
- ...use "**NS(Name Server) Records**" to point at the **NS** (connect to global DNS)
- Resource Records (**RR**) created within the Hosted Zone
- externally registered domains can point at R53 Public Zone (e.g. use GoDaddy to register a domain, add those name servers into the DNS system)

PublicHostedZones-1.png

### R53 Private Hosted Zones

- a **public** hosted zone... **which isn't public**
- associated with **VPCs**
- ...**only accessible** in those **VPCs**
- ...using **different accounts** is supported Via **CLI**/**API**
- split-view (overlapping **public** & **private**) for **PUBLIC** and **INTERNAL** use with the same zone name

PrivateHostedZones-1.png

SplitView.png
