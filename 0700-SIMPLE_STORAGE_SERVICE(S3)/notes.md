# SIMPLE STORAGE SERVICE (S3)

### S3 Security (Resource Policies & ACLs)

- S3 is private **by default**
  - default only the account root user of the account which owns that bucket
- S3 Bucket Policies
  - a form of resource policy
  - like identity policies, but attached to a bucket
  - resource perspective permissions (who can access that resource)
  - allow/deny same or **different** accounts
  - allow/deny **anonymous** principals

![Screen Shot 2022-10-30 at 2.55.44 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21ae2ac7-d753-4af1-b680-630a99623483/Screen_Shot_2022-10-30_at_2.55.44_PM.png)

- Access Control Lists (ACLs)
  - ACLs on **objects** and **bucket**
  - a subresource
  - legacy
  - inflexible & simple permissions

![Screen Shot 2022-10-30 at 3.03.53 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99faf172-af62-497a-a034-e34e5a688ac7/Screen_Shot_2022-10-30_at_3.03.53_PM.png)

- a fail-safe

_Summary_

![Screen Shot 2022-10-30 at 3.04.55 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b48a326f-a67c-43c1-9f00-6f0d2476ea33/Screen_Shot_2022-10-30_at_3.04.55_PM.png)

### S3 Static Hosting

- normal access is via **AWS APIs**
- this feature allows access via HTTP - e.g. Blogs…
- **Index** and **Error** documents are set
- **Website Endpoint** is created
- Custom Domain via **R53** - **BUCKETNAME MATTERS**

### Object Versioning & MFA Delete

- controlled at a bucket level
- start of in a disabled state
- once enabled, can’t disable it again, but can suspend it

  ![Screen Shot 2022-10-30 at 3.35.16 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1abb8f4f-3405-49f3-b0a0-4015e84741fd/Screen_Shot_2022-10-30_at_3.35.16_PM.png)

- versioning lets you store **multiple versions** of objects within a bucket. Operations that would modify objects **generate a new version**

  ![Screen Shot 2022-10-30 at 3.39.30 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ca0ad95-c557-494c-8007-83aaa1079db9/Screen_Shot_2022-10-30_at_3.39.30_PM.png)

  To permanently delete, specify the id value

  ![Screen Shot 2022-10-30 at 3.40.01 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8a20c30-60f7-46b0-9f16-f274d57dccd3/Screen_Shot_2022-10-30_at_3.40.01_PM.png)

- …**cannot be switched** off - only suspended
- space is consumed by ALL versions
- you are billed for ALL versions
- Only way to 0 costs - is to delete the bucket

_MFA Delete_

- enabled in **versioning configuration**
- MFA is required to change bucket **versioning state**
- MFA is required to **delete versions**
- Serial number (MFA) + Code passed with API CALLS

### S3 Performance Optimization

- each upload to S3 a single data stream to
- stream fails - **upload fails**
- requires full restart to recover
- speed & reliability = limit of 1 stream
- any upload up to **5GB**…

\*Solution - **Multipart Upload\***

- data is broken up
- min data size **100MB** for multipart
- **10,000** max parts, each part **5MB** → **5GB**
- last part can be smaller than 5MB
- parts can fail, and be restarted in isolation
- transfer rate = speeds of all parts

_S3 Accelerated Transfer (Off)_

- uses the network of AWS edge locations
- default is switched Off
- restrictions to turn it on
  - no periods in bucket name
  - DNS compatible in its naming
- improves transfer the larger the distance between the upload location and location of S3 bucket

### Key Management Service (KMS)

- regional & public service
- create, store and manage keys
- **symmetric** and **asymmetric** keys
- cryptographic operations (**encrypt**, **decrypt**, &…)
- **Keys never leave KMS** - Provides FIPS 140-2(**L2**) : a security standard
- main type of key KMS manages are **KMS Keys** - sometimes referred as CMKs (customer master key)
  - you, applications or other AWS services can use them
  - KMS keys are **logical** - ID, date, policy, desc & state
  - … backed by **physical** key material
  - generated or imported by KMS
  - KMS Keys can be used up for up to 4KB of data

_Key Concepts_

- KMS Keys are isolated to a **region** & never leave
- also supports multi-region keys discussed (if required) in a different video
- keys are either AWS owned or Customer Owned
- customer owned two types - **AWS** Managed or **Customer** Managed KEYS
- AWS managed keys are created automatically by AWS when you use a service such as S3
- customer managed keys are created by the customer directly in an application or within an AWS service
- customer manage keys are more configurable
- KMS Keys support rotation
- contains **backing key** (and **previous** backing keys)
- can create aliases - shortcuts to keys
  - per region

![Screen Shot 2022-11-02 at 5.36.15 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a8dc953-5dc0-4344-a796-3ca85f2e5547/Screen_Shot_2022-11-02_at_5.36.15_PM.png)

### Object Encryption

- buckets aren’t encrypted… **objects** are…
- two types of encryption
  - **client**-side encryption - objects are encrypted by the client before it’s sent
  - **server**-side encryption - objects are encrypted by the S3 infrastructure
  - encryption at rest (when stored in disk) not network (comes standard with S3)
- three types of server-side encryption available in S3
  - Server-Side Encryption with Customer-Provided Keys (**SSE-C**)
  - Server-Side Encryption with Amazon S3-Managed Keys (**SSE-S3**)
  - Server-Side Encryption with KMS KEYS Stored in AWS Key Management Service (**SSE-KMS**)

### S3 Lifecycle Configuration

- can create rules on S3 Buckets which can automatically transition or expire objects in the bucket
- great way to optimize the cost for larger S3 buckets
- a lifecycle configuration is a **set of rules**
- rules consist of **actions**…
- … apply on a **Bucket** or **groups of objects**
- **Transition** Actions - change the storage class of whichever object or objects are affected
- **Expiration** Actions - can delete whatever object or objects or object versions are affected

![Screen Shot 2022-11-07 at 3.15.14 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfb4cb95-3813-4b06-ad34-56cd11c4cb87/Screen_Shot_2022-11-07_at_3.15.14_PM.png)

### S3 Replication

- Cross-Region Replication (**CRR**)
- Same-Region Replication (**SRR**)
- differs depending on whether the buckets are in the same AWS accounts or different AWS accounts

![Screen Shot 2022-11-07 at 3.22.56 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8f86122-0dc9-4c6f-b31d-49e5e6fad252/Screen_Shot_2022-11-07_at_3.22.56_PM.png)

_S3 Replication Options_

- **All objects** or a **subset**
- **Storage Class** - default is to maintain
- **Ownership** - default is the source account
- Replication Time Control (**RTC**) - required if objects need to be in sync in less than **15** **mins**

_S3 Replication Considerations_

- **Not retroactive** & Versioning needs to be **ON**
- **One-way replication** Source to Destination
- Unencrypted, SSE-S3 & SSE-KMS (with extra config)
- Source bucket owner needs permissions to objects
- NO **system events**, **Glacier** or **Glacier Deep Archive**
- **NO DELETES**

_Why use replication…?_

- SSR
  - Log Aggregation e.g. if you got multiple different S3 buckets that store logs for different systems
  - PROD and TEST Sync
  - Resilience with strict sovereignty (e.g. some data can not leave a specific AWS region because of requirements)
- CRR
  - Global Resilience Improvements
  - Latency Reduction

### S3 Predesigned URLs

- a way to give another person or application access to an object inside an S3 bucket using your credentials in a safe and secure way

![Screen Shot 2022-11-07 at 4.29.16 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e04f32-52e3-43c4-9a36-ebe3e1cdb2f2/Screen_Shot_2022-11-07_at_4.29.16_PM.png)

![Screen Shot 2022-11-07 at 4.29.34 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e47763e-e0f4-49aa-86d8-768c25f2eeca/Screen_Shot_2022-11-07_at_4.29.34_PM.png)

![Screen Shot 2022-11-07 at 4.29.55 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ebbbccf-4a2f-480e-be51-7c294afb1dde/Screen_Shot_2022-11-07_at_4.29.55_PM.png)

_Exam PowerUP!_

- you can create a URL for an object you have **no access to**
- when using the URL, the permissions match the **identity which generated it**…
- access denied could mean the generating ID **never had access** or **doesn’t have access right now**
- \*\*\*\*… URL stops working when temporary credentials expire…

### S3 Select and Glacier Select
