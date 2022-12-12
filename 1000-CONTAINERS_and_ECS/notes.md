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
