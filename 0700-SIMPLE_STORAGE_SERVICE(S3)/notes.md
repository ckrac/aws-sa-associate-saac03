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

- Access Control Lists (ACLs)

  - ACLs on **objects** and **bucket**
  - a subresource
  - legacy
  - inflexible & simple permissions

- a fail-safe

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

- versioning lets you store **multiple versions** of objects within a bucket. Operations that would modify objects **generate a new version**

  To permanently delete, specify the id value

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

### S3 Replication

- Cross-Region Replication (**CRR**)
- Same-Region Replication (**SRR**)
- differs depending on whether the buckets are in the same AWS accounts or different AWS accounts

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

### S3 PreSigned URLs

- a way to give another person or application access to an object inside an S3 bucket using your credentials in a safe and secure way

_Exam PowerUP!_

- you can create a URL for an object you have **no access to**
- when using the URL, the permissions match the **identity which generated it**…
- access denied could mean the generating ID **never had access** or **doesn’t have access right now**
- \*\*\*\*… URL stops working when temporary credentials expire…

### S3 Select and Glacier Select

- are ways to retrieve parts of objects, rather than the entire object
- you often want to retrieve the **entire object**
- retrieving a 5TB object... **takes time, uses 5TB**
- filtering at the client side **doesn't reduce this**
- S3/Glacier select let you use SQL-Like statements to select part of the object, **pre-filtered by S3**
- CSV, JSON, Parquet, BZIP2 compression for CSV and JSON

### S3 Events

- allows to create event notification configurations on a bucket
- **notification** generated when **events** occur in a **bucket**
- can be delivered to **SNS**, **SQS** and **Lambda** Functions
- object **created** (Put, Post, Copy, CompleteMultiPartUpload)
- object **delete** (\*, Delete, DeleteMarkerCreated)
- object **restore** (Post (Initiated), Completed)
- **replication** (OperationMissedThreshold, OperationReplicatedAfterTHreshold, OperationNotTracked, OperationFailedReplication)

### S3 Access Logs

(in learning aid)

### S3 Object Lock

- object lock enabled on 'new' buckets (Support req for existing)
- write-once-read-many (**WORM**) - **No Delete, No Overwrite**
- requires **versioning** - **individual versions** are locked
- can have **Both**, **One**, or **the other**, or **none**
  - 1 - **Retention** Period
    - specify **DAYS** & **YEARS** - A Retention Period
    - **COMPLIANCE** mode - **Can't** be **adjusted**, **deleted**, **overwritten**
      -... **even by the account root user**
      - **until retention expires**
      - might use for legal issue like for drug data
    - **GOVERNANCE** mode - special **permissions** can be granted **allowing lock settings to be adjusted**
      - **s3:BypassGovernanceRetention**
      - ... **x-amz-bypass-governance-retention:true** (console default)
      - useful to prevent accidental deletion, process reasons or governance reasons to keep object versions, test of settings before picking the compliance mode
  - 2 - **Legal Hold**
    - set on an **object version** - **ON** or **OFF**
    - no retention
    - **NO Deletes** or **Changes** until removed
    - **s3:PutObjectLegalHold** is required to add or remove
    - prevent accidental deletion of critical object versions
- a bucket can have **default Object Lock Settings**
