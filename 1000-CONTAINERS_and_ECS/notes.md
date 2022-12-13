# CONTAINERS & ECS

### Introduction to Containers

Virtualisation Problems - Containers101-1.png

Containerization - Containers101-2.png

Image Anatomy - Containers101-3.png

Container Anatomy - Containers101-4.png

Container Registry - Containers101-5.png

- **dockerfiles** are used to **build images**
- portable - self-contained, always run as expected
- lightweight - parent OS used, **fs layers are shared**
- container only runs the application & environment it needs
- provides much of the isolation VM's do
- ports are '**exposed**' to the host and beyond
- application stacks can be multi-container

### Elastic Container Service (ECS) Concepts

- ECS is a service that accepts containers and instructions that you provide and it orchestrates where and how to run those containers

ECS-Concepts.png

- to tell ECS about your container images, you create a
  - container definition
    - tells ECS where your container image is
    - tells which port your container uses
  - task definitions
    - represents a self-contained application, the application as a whole (stores whatever container definitions are used to make up that one single application)
    - a task could have one container defined inside it or many
    - store the
      - resources used by that task (CPU and memory),
      - networking mode,
      - compatibility (wether it works on ec2 mode or fargate),
      - task role (IAM role that a task can assume)
  - ECS service (service definition)
    - configure that via service definition
    - defines a service and as service is how for ECS. we can define how we want a task to scale (ie. how many copies we'd like to run)
    - it can add capacity and resilience because we can have multiple independent copies of our task running and you can deploy a load balancer in front of a service

_Concepts_

- **container definition** - image & ports
- **task definition** - security (task role), container(s), resources
- **task role** - IAM Role which the TASK assumes
- **service** - how many copies, high availability, restarts

### ECS - Cluster Mode

ECS-ClusterMode-[1-2].png

EC2 vs ECS (EC2) vs Fargate

- if you use containers ... use **ECS**
- **large** workload - **price** conscious - **EC2 Mode**
- **large** workload - **overhead** conscious - **Fargate**
- **small** / **burst** workloads - **Fargate**
- **batch** / **periodic** workloads - **Fargate**

### Elastic Container Registry (ECR)

- managed **container image registry** service
- like **docker hub** but for **AWS**
- each **AWS** account has a **public** and **private** registry
- each **registry** can have many **repositories**
- each **repository** can contain many **images**
- **images** can have several **tags**
- **public** = public R/O ... R/W requires permissions
- **private** = permissions required for any R/O or R/W
- integrated with **IAM** - **Permissions**
- image scanning, **basic** and **enhanced**(inspector) - scans issues with both operating systems and any software packages within your containers
- nr real-time **Metrics** => **CW** (auth, push, pull)
- **API** actions = **CloudTrail**
- **events** => **EventBridge**
- replication... cross-region and cross-account

### Kubernetes 101

- open source container system use to automate the deployment, scaling, and management of containerized applications

Kubernetes101-[1-2].png

_Summary_

- **cluster** - a deployment of Kubernetes, management, orchestration...
- **node** - resources: pods are placed on nodes to run
- **pod** - 1+ containers; smallest unit in kubernetes; often 1 container 1 pod
- **service** - abstraction, service running on 1 or more pods
- **job** - ad-hoc, creates one or more pods until completion
- **ingress** - exposes a way into a service (**Ingress** => **Routing** => **Service** => 1+ **Pods**)
- **ingress controller** - used to provide ingress (e.g. AWS LB Controller uses ALB/NLB, NGINX, etc.)
- **job** - ad-hoc, creates one or more pods until completion
- **persistent** storage (**PV**) - volumes whose lifecycle lives beyond any 1 pod using it

### Elastic Kubernetes Service (EKS) 101

-
