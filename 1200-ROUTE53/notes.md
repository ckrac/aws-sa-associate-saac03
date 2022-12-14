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

### CNAME vs R53 Alias

_Problems using only CNAME_

- "A" maps a **NAME** to an **IP Address**
- ...**catagram.io** => **1.3.3.7**
- **CNAME** maps a **NAME** to another **NAME**
- ...**www.catagram.io** => **catagram.io**
- CNAME is **invalid for naked/apex** (catagram.io)
- many AWS services use a DNS Name (ELBs - elastic load balancer don't give an ip address but a DNS name)
- with just CNAME - **catagram.io** => **ELB would be invalid**

_ALIAS Records_

- **ALIAS** records map a **NAME** to an **AWS resource**
- can be used both for **naked**/**apex** and **normal** records
- for non apex/naked - functions like **CNAME**
- there is no charge for **ALIAS** requests pointing at **AWS resources**
- for **AWS Services** - **default** to picking **ALIAS**
- should be the same "Type" as what the record is pointing at
- API Gateway, CloudFront, Elastic Beanstalk, ELB, Global Accelerator & S3

### Simple Routing

- Simple routing lets you configure standard DNS records, with no special Route 53 routing such as weighted or latency. With simple routing, you typically route traffic to a single resource, for example, to a web server for your website

Route53SimpleRouting-1.png

### R53 Health Checks

- health check are **separate from**, but are **used by** records
- health checkers located **globally**
- health checkers check every 30s (every 10s costs extra)
- can be TCP, HTTP/HTTPS, HTTP/HTTPS with String Matching
- endpoint is either **Health** or **Unhealthy**
- can be Endpoint, CloudWatch Alarm, Checks of Checks (Calculated) checks

Route53HealthChecks.png

### Failover Routing

- failover routing lets you route traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy

Route53FailoverRouting-1.png

### Multi Value Routing

- Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources

Route53MultiValueRouting-1.png

### Weighted Routing

- Weighted routing lets you associate multiple resources with a single domain name (catagram.io) and choose how much traffic is routed to each resource. This can be useful for a variety of purposes, including load balancing and testing new versions of software.

Route53WeightedRouting-1.png

### Latency Routing

- If your application is hosted in multiple AWS Regions, you can improve performance for your users by serving their requests from the AWS Region that provides the lowest latency.

Route53LatencyBasedRouting-1.png

### Geolocation Routing

- Geolocation routing lets you choose the resources that serve your traffic based on the geographic location of your users, meaning the location that DNS queries originate from.

Route53GeolocationRouting-1.png

### Geoproximity Routing

- Geoproximity routing lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.

Route53GeoProximityRouting-1.png

### R53 Interoperability

- using R53 to register domains or to host zone files, when the other part is not with R53
- R53 normally has 2 jobs - **Domain registrar** and **Domain Hosting**
- R53 can do **BOTH**, or either **Domain registrar** or **Domain Hosting**

What happens when you register a domain using R53?

- R53 accepts your money (**domain registration fee**)
- R53 allocates 4 name servers (NS) (**domain hosting**)
- R53 creates a zone file (**domain hosting**) on the above NS
- R53 communicates with the registry of the TLD (**Domain Registrar**)
- ...sets the NS records for the domain to point at the 4 NS above

Route53Interoperability-[1-3].png
