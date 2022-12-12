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
