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
