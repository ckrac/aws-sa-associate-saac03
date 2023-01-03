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
