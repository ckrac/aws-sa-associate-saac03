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

https://en.wikipedia.org/wiki/File_system_permissions

https://docs.aws.amazon.com/efs/latest/ug/performance.html

https://docs.aws.amazon.com/efs/latest/ug/storage-classes.html

https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html

### AWS Backup

- use AWS Backup to centralize and automate data protection across AWS services and hybrid workloads
- AWS Backup offers a cost-effective, fully managed, policy-based service that further simplifies data protection at scale. AWS Backup also helps you support your regulatory compliance or business policies for data protection
- together with AWS Organizations, you can use AWS Backup to centrally deploy data protection policies to configure, manage, and govern your backup activity across your company’s AWS accounts and resources

AWSBackup.png

- **backup plans** - frequency, window, lifecycle, vault, region copy
- **resources** - what resources are backed up
- **vaults** - backup **destination** (container) - assign KMS key for encryption
- vault **lock** - write-once, read-many (**WORM**), 72 hour cool off, then even AWS can't delete
- **on-demand** - manual backups created as needed
- **PITR** - point in time recovery

https://aws.amazon.com/backup-restore/services/
