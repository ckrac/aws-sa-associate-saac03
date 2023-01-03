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
