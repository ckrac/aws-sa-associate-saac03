# HA & Scaling

### Regional and Global AWS Architecture

- Global **Service Location** & **Discovery**
  - e.g when you type netflix.com into your browser, how does your machine discover where to point at?
- Content Delivery (**CDN**) and optimization
  - how does the content or data for an application get to users globally?
- Global **health checks** & **Failover**
  - detecting if infrastructure in one location is healthy or not and moving customers to another country as required
- regional **entry point**
- **scaling** & **resilience**
- application services and **components**

RegionalandGlobalArchitecture-[1-2].png

### Evolution of the Elastic Load Balancer

- 3 Types of load balancers (ELB) available within AWS
- split between **v1** (**avoid**/**migrate**) and **v2** (**prefer**)
- classic load balancer (**CLB**) - **v1** - introduced in 2009
  - not really layer 7, lacking features, **1 SSL per CLB**
- application load balancer (**ALB**) - v2 - HTTP/S/WebSocket
- network load balancer (**NLB**) - v2 - TCP, TLS & UDP
- V2 = faster, cheaper, support target groups and rules

### Elastic Load Balancer Architecture

- it's the job of the load balancer to accept connections from customers and then to distribute those connections across any registered backend compute
  - the user is abstracted away from the physical infrastructure
  - amount of infrastructure can change (increase or decrease in number) without affecting customers

ELB-[1-3].png

- ELB is a **DNS A** Record pointing at **1+** Nodes per AZ
- nodes (in one subnet per AZ) can scale
- **internet-facing** means nodes have **public IPv4 IPs**
- **internal** is **private only IPs**
- EC2 **doesn't need to be public** to work with a LB
- **listener** configuration controls **WHAT** the LB does
- **8+** Free IPs per subnet, and **/27** subnet to allow scaling

### Application Load Balancing (ALB) vs Network Load Balancing (NLB)

ALBvsNLB-1.png

_Application Load Balancer (**ALB**)_

- **Layer 7** load balancer... listens on **HTTP** and/or **HTTPS**
- **no other Layer 7 protocols** (SMTP, SSH, Gaming...)
- ...and **NO TCP/UDP/TLS Listeners**
- L7 content type, cookies, custom headers, user location and app behavior
- HTTP HTTPS (SSL/TLS) always terminated on the ALB - **no unbroken SSL** (security teams!)
- ...**a new connection** is made to the application
- ALBs **MUST** have **SSL** certs if **HTTPS** is used
- ALBs are **slower** than **NLB**... more levels of the network stack to process
- health checks **evaluate application health**... layer 7

_Application Load Balancer (**ALB**) - Rules_

- rules **direct connections** which **arrive** at a **listener**
- processed in **priority order**
- **default rule** = **catchall**
- **rule conditions**: host-header, http-header, http-request-method, path-pattern, query-string & source-ip
- **actions**: forward, redirect, fixe-response, authenticate-oidc & authenticate-cognito

ALBvsNLB-2.png

_Network Load Balancer (**NLB**)_

- **Layer 4 load balancer**... **TCP**, **TLS**, **UDP**, **TCP_UDP**
- **no visibility** or **understanding** of **HTTP** or **HTTPS**
- **no headers**, **no cookies**, **no session stickiness**
- really really really fast (**millions of rps**, **25%** of **ALB latency**)
- ...SMTP, SSH, Gamer Servers, financial apps (**not http/s**)
- health checks **JUST** check ICMP/TCP Handshake... **not app aware**
- NLB's can have **static IP's** - useful for whitelisting
- **forward TCP** to instances... **unbroken encryption**
- used with private link to provide services to other VPCs

_**ALB** vs **NLB**_

- unbroken encryption... NLB
- static IP for whitelisting... NLB
- the fastest performance... NLB (millions rps)
- protocols not HTTP or HTTPS... NLB
- privatelink... NLB
- otherwise... ALB

### Launch Configuration and Templates

- allow you to define the configuration of an EC2 instance **in advance**
- AMI, Instance Type, Storage & Key pair
- networking and security groups
- userdata & IAM role
- both are NOT editable - defined once. LT has versions
- LT provide **newer features** - including T2/T3 Unlimited, Placement Groups, Capacity Reservations, Elastic Graphics

LCandLT.png

### Auto-Scaling Groups

- **automatic scaling** and **self-healing** for EC2
- uses **launch templates** or **configurations**
- has a **minimum**, **desired** and **maximum** size (e.g. 1:2:4)
- keep running instances at the **desired capacity** by **provisioning** or **terminating** instances
- **scaling policies** automate based on metrics

ASG-[1-2].png

_Scaling Policies_

- **manual** scaling - manually adjust the desired capacity
- **scheduled** scaling - time based adjustment - e.g. sales...
- **dynamic** scaling
  - **simple** - "CPU above 50% + 1", "CPU below 50% - 1"
  - **stepped** - bigger +/- based on difference
  - **target tracking** - desired aggregate CPU = 40%... ASG handle it
- **cooldown periods** - value in seconds, how long to wait at the end of a scaling action before doing another

_ASG + Load Balancers_

ASG-3.png

_Scaling Processes_

- **launch** and **terminate** - SUSPEND and RESUME
- **AddToLoadBalancer** - add to LB on launch
- **AlarmNotification** - accept notification from CW
- **AZRebalance** - balances instances evenly across all of the AZs
- **HealthCheck** - instance health checks on/off
- **ReplaceUnhealthy** - terminate unhealthy and replace
- **ScheduledActions** - schedule on/off
- **Standby** - use this fir instances 'InService vs Standby'

_Final points_

- autoscaling groups are free
- only the resources created are billed...
- use cool downs to avoid rapid scaling
- think about **more**, **smaller** instances - **granularity**
- use with ALB's for elasticity - **abstraction**
- ASG defines **WHEN** and **WHERE**, LT defines **WHAT**

### ASG Scaling Policies

- ASGs don't NEED scaling policies - they can have none
- **manual** - **min**, **max** & **desired** - testing & urgent
- **simple scaling**
- **step scaling**
- **target tracking**
- scaling based on **SQS** - **ApproximateNumberOfMessagesVisible**

ASG-Policies-[1-2].png
