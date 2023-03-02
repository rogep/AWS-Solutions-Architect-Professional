# Advanced Identitites & Federation

## SAML2.0 Identity Federation (Security Assertion Markup Language)

The process of using an identity from another provider to access AWS **indirectly**.

- Enterprise Identity Provider that is **SAML2.0** compatible
- Existing identity management team
- **Single source of truth** or more than **5000 users**
- Uses **IAM Roles** and **AWS Temp Credentials (12 hours)** -- requires a bidirectional trust
- **STS:AssumeRoleWithSAML**
- Typically used with a Windows based identity provider

## IAM Identity Center (Successor to AWS Single-Sign-On)

AWS recommends you to use this service now

- Manage SSO Access - **AWS Accounts** and **External Applications**
- Flexible **Identity Source**
- **Preferred by AWS** vs traditional '**workforce**' identity federation for **AWS accounts** and **external applications**
- AWS Managed Microsoft AD
- Centralised permissions managements (users/role based) with **permission sets**
- Always default to this for **workplace** applications and then analyse the question further
- Requires an Organisation to be created

## Cognito User and Identity Pools

The service as a whole provides **authentication**, **authorisation** and **user management** for web/mobile apps. Note that cognito has misleading names!

- **USER POOL**: Sign-in and get a JSON Web Token (JWT) - most aws services can't use JWT's
- User directory manage and profiles, sign-up and sign in (customisable web ui), MFA and other security features
- Imagine a database of users that sign in and receive a JWT
- **IDENTITY POOL**: Allow you to offer access to **temporary AWS Credentials**
- Unauthenticated identities - guest users
- federated identities - SWAP/Google/FB/Twitter/SAML2.0 & user pool for short term AWS creds to access AWS recoursews
- Assuming an IAM role and are configured in identity pools

Web identity federation is using a combination of user pools and identity pools.

- The sign-in from external providers are mapped to a user pool (returns a JWT token)
- Pass the JWT token to an identity pool and now we can assume roles to access sources
- This process offers unlimited number of users

## Amazon Workspace

- Desktop-As-A-Service (DAAS) - Home-working/Office
- Similar to Citrix/Remote Desktop - Hosted by AWS
- Consistent desktop from anywhere - apps and state
- Windows and linux - Various sizes
- Charged either monthly or hourly -- minimum month base infrastructure cost
- Uses Directory service (Simple, AD, AD connector) for authentication and user management
- Workspaces use an **Elastic Network Interface (ENI)** in a VPC -- **use VPC networking**
- Or on-premises resources over VPN or Direct connect
- At-rest encryption (EBS + KMS)
- Not highly available by design; like EC2 they occupy a single subnet/AZ

## AWS Directory Services (Microsoft Active Directory-as-a-Service)

- Built using Microsoft Active Directory 2012 RS
- Managed using Standard Active Directory tools
- Supports Group policy and SSO
- Supports Schema extension - MS AD Aware Apps
- Sharepoint, SQL, Distributed file system (DFS)
- Two sizes: Standard (30,000) and Enterprise (500,000)
- Used for AD Authentication/authorisatoin of products and services within AWS
- Highly-Available by default (2AZ+)
- Supports one-way and two-wayu external and forest trusts with on-premise active directory
- Directory in AWS - Can operate through a network link failure to any connected on-premises systems\*\*
- Supports RADIUS-based MFA
- Best choice for more than 5000 users and if you need trust relationships between AWS and youyr on-premises directories\*\*
- Automatic patching and maintanence
- If you don't want any directory data in AWS you want to use Microsoft AD Connector\*\*

### AD Connector

- A pair of directory endpoints running in AWS (ENIs in a VPC)
- Redirects requests to an existing directory servers
- No directory data stored in AWS - all redirected
- Uses existing on-premises AD with directory compatible AWS Services - without any identity data in AWS
- Two sizes: small and large -- no user limit just controls the amount of compute allocated -- throughput limits
- Connector requires 1 of more directory server(s) to be configured
- REQUIRES a working network connection
- Network connectivity via Direct Connect or VPN

## AWS Control Tower

- Quick and Easy setup of multi-account environment
- Orchestrates other AWS services to provide this functionality (including organisations)
- Organisations, IAM identity center, CloudFormation, Config and more...
- Landing Zone - Multi-account environment
- SSO/ID Federation, Centralised Logging and Auditing
- Guard Rails - Detect/Mandate rules/standards across all accounts
- Account Factory - Automates and Standardises new account creation
- Dashboard - single page oversight of the entire environment
