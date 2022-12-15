# ADVANCED EC2

### Bootstrapping EC2 using User Data

- bootstrapping allows **EC2 Build Automation**
- enabled using user data - accessed via the meta-data IP
- http://169.254.169.254/latest/user-data
- anything in User Data is **executed** by the **instance OS**
- executed **ONLY on Launch** (first initial launch)
- EC2 doesn't interpret, the OS needs to understand the User Data

EC2Bootstrapping-1.png

_User Data Key Points_

- it's **opaque** to EC2... its just a **block of data**
- it's **NOT** secure - don't use it for passwords or long term credentials (ideally)
- User Data is limited to 16 KB in size
- can be modified when instance stopped
- but **only executed once** at launch

How quick is it ready for service?

EC2Bootstrapping-2.png

### Enhanced Bootstrapping with CFN-INIT

- a way to pass complex bootstrapping instructions into an EC2 instance
- **cfn-init** helper script - installed on EC2 OS
- simple configuration management system
- procedural (user data) vs desired stater (cfn-init)
- can update packages, groups, users, sources, files, commands and services
- provided with directives via **Metadata** and **AWS::CloudFOrmation::Init** on a CFN resource

CFN-INIT-[1-2].png

### EC2 Instance Roles & Profile

EC2InstanceRoles-1.png

- credentials are inside meta-data
- iam/security-credentials/**role-name**
- automatically rotated - always valid
- should always be used rather than adding access keys into instance
- CLI tools will use ROLE credentials automatically

### SSM Parameter Store

- the SSM Parameter store is a service which is part of Systems Manager which allows the storage and retrieval of parameters - string, stringlist or secure string
- the service supports encryption which integrates with KMS, versioning and can be secured using IAM
- the service integrates with many AWS services and can be accessed using the CLI/APIs from anywhere with access to the AWS Public Spare Endpoints

- storage for **configuration** & **secrets**
- string, stringlist & secure string
- license codes, database strings, full configs & passwords
- hierarchies & versioning
- plaintext and ciphertext
- public parameters - **Latest AMIs per region**

SSMParameterStore.png
