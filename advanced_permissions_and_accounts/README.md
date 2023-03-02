# Advanced Permissions & Accounts

## Organisations

- An organisation account needs to be created in a standalone AWS account and this account is called the **management** account
- Existing accounts need to approve an invite to an organisation and then become member accounts
- Hierarchical structure (inverted tree) can contain accounts (Organisation Units)
- Consolidated billing is the payment account from the management account
- Monthly billing and reservations and volume discounts if used (some services are cheaper)
- Can create new accounts directly within an org (no invite process) -- just need a unique email `email+<unique>@email.com`

## Service Control Policies (SCP)

- SCPs are **account permission boundaries**
- They limit what the account (including the account root user) can do
- Attaching service control policies (SCP) to a single unit it only affects that unit. If you connect it to an OU it traverses down to all OU
- Management account is not affected by SCP
- Good for restricting what regions are used or EC2 compute
- They **don't grant** permissions, but define a boundary
- Think **allow** or **deny** lists (deny/allow/deny)
- Only things inside an Identity policy and SCP are allowed

## Security Token Service (STS)

A user/identity assumes a role via STS which then checked the permissions policy attached to the role. STS generates **temporary credentials** which can access AWS resources until **expiration**.

- Generates temoporary credentials (`sts:AssumeRole*`)
- Simmilar to access keys, however they expire and don't belong to the identity
- Requested by an identity (AWS or EXTERNAL)
- When credentials expire, another `assumeRole` call is required

### Revoking IAM Role Temporary Security Credentials

- Roles can be assumed by **many identities**
- Temport credentials **cannot be cancelled**

If credentials are leaked, changing the **TRUST** policy has **no impact** on **existing** credentials

- Updating permissions policy impacts **ALL** credentials
- `AWSRevokeOlderSessions` inline **DENY** for any sessions older than **NOW**
- This conditional **DENY** is added to the permissions policy

We can extract these credentials from inside an EC2 instance by doing

- `curl http://169.254.16 9.254/latest/meta-data/iam/security-credentials/` and this will return the Role that you it is using
- Append the role to the above command `curl http://169.254.169.254/latest/meta-data/iam/security-credentials/A4L-InstanceRole-1JWDXJ4HSCGJV` and this will expose the security credentials in plaintext
- To revoke access to this role go to `iam role > role > revoke sessions`
- For EC2 services, you will need to stop/start again as they will need to use a new stsToken

## Permission Boundaries & Use-cases

- Only can be applied to IAM users and roles
- Don't **GRANT** any access on their own. The define a **maximum permissions** an **identity** can receive
- Great for permission delegation

## Permissions Evaluation

In order from most important to least checked by AWS

1. Explicit Deny
2. SCPs (AWS account specific)
3. Resource Policies
4. Permissions Boundaries
5. Session Policies
6. Identity Policies

## AWS Resource Access Manager (RAM)

Allows you to share resources **between AWS accounts**

- Products need to support RAM
- Shared with **Principals** (accounts, OU's or ORG)
- Shared resources can be accessed **natively**
- No cost associated with RAM, but only with the resources used
- Owner account **creates a share**, provides a name
- Owner **retains full ownership**
- Defined the **principal** with **whome to share**
- If principal is inside an org, all acconts are automatically shared
- Some resources can be shared with **ANY** account, some **only with AWS ORG accounts**
- You can only see the principal and not other accounts that are shared

Availability zone ID's are consistent accoess accounts, but availability zones are not

## Service Quotas

- Each service has a **default per-region** quota
- Some services can be **per account**
- Most service quotas **can be increased** as needed
- Some can't be changed (IAM users 5000) -- find this list
- Can define a **quota request template**
- Can create a **cloudwatch alarm** based on a service quota

https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html

https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/

https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/template

https://docs.aws.amazon.com/servicequotas/latest/userguide/organization-templates.html

https://docs.aws.amazon.com/servicequotas/latest/userguide/configure-cloudwatch.html

https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/list-service-quotas.html

https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/list-aws-default-service-quotas.html

https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html
