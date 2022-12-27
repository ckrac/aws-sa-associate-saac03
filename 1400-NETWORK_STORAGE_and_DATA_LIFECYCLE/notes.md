# Network Storage and Data Lifecycle

### EFS Architecture

- Elastic File System (EFS) is an AWS managed implementation of NFS which allows for the creation of shared 'filesystems' which can be mounted within multi EC2 instances
- **EFS** is an implementation of **NFSv4**
- EFS **Filesystems** can be mounted in **Linux**
- **shared** between many EC2 Instances
- private service, via **mount targets** inside a VPC
- can be accessed from on-premises - **VPN** or **DX**

EFS-1.png

- **Linux only**
- **general purpose** and **Max I/O** performance modes
- general purpose - **default** for 99.9% of uses
- **bursting** and **provisioned** throughput modes
- **standard** and **infrequent access (IA)** Classes
- lifecycle policies can be used with classes
