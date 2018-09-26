AWS Certified Developer Associate -  Notes
=====================================

## VPC
* Three subnet types: Private, Public and VPN
* Single Region, multi AZs
* Security Groups:
    * Resources level traffic firewall (EC2 instance, ELB, etc..)
    * Ingress and Egress
    * Stateful
* Access Control Lists:
    * Source and Protocol filtering
    * Subnet level trafic firewall
    * Stateless
### NAT Instances vs NAT Gateways
| NAT Instances                                        | NAT Gateways                                                 |            
| ---------------------------------------------------- |:------------------------------------------------------------:| 
| Use script to manage fail over between instances     | Highly available, are implement with redundancy in each AZs  |
| Depends on the bandwidth of intance type             | Is a service                                                 |
| Manage by you                                        | Managed by AWS                                               |
| A generic AMI that's configured to perform NAT       | Software is optimized for handling NAT traffic               |
| Manual port fordwarding                              | Port fordwarding NOT supported                               |
| Use a bastion server                                 | Bastion server not supported                                 |
| View CloudWatch alarms                               | Traffic metrics not supported                                |

### VPC Peering
* Single region Inter-VPC routing
* Connection between same or different AWS account
* DNS supported


### VPC Access types
| VPN                                        | Gateways                                         |            
| ------------------------------------------ |:------------------------------------------------:| 
| Hardware-based VPN  (w/ port redundancy)   | Internet Gateway (IGW)                           |
| Direct Connect                             | Virtual Private Gateway                          |
| VPN CloudHub                               | Customer Gateway                                 |   
| Software VPN                               | Software is optimized for handling NAT traffic   |

#### Direct Connect
* Predictable bandwidth
* Predictable performance/consistent network experience
* Support for VLAN Trunking (802.1Q)
* Can be partitioned into multiple Virtual Interfaces
#### AWS VPN CloudHub
* Direct connection to VPC for Branch offices 
### Warnings
* Subnets do not span over AZs
* Update the inbound or outbound rules for your VPC Security Groups to reference Security Groups in the peered VPC
* VPC Peering: Can't overlap network addresses
* Direct Connect: 
    * Bandwidth of 1 Gbps or 10 Gbps
    * Performance and bandwidth depends on distance of AWS Region / Edge Router


### Limits
* The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use, and cannot be assigned to an instance. For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:
    
   * 10.0.0.0: Network address.
    
   * 10.0.0.1: Reserved by AWS for the VPC router.
    
   * 10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see Amazon DNS Server.
    
   * 10.0.0.3: Reserved by AWS for future use.
    
   * 10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.
* CIDR : 16-28
* VPC Peering: 50 VPC Peers per VPC, up to 125 by request  

## IAM
* AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.
#### Features at a glance
* Shared access to your AWS account
* Granular permissions
* Secure access to AWS resources for applications that run on Amazon EC2
* Multi-factor authentication (MFA)
* Identity federation 
* PCI DSS Compliance
* Integrated with many AWS services
* Eventually Consistent
* Free to use
#### Accesing IAM
* AWS Management Console
* AWS Command Line Tools
* AWS SDKs
* IAM HTTPS API



### Understanding How IAM Works

The IAM infrastructure includes the following elements:
* **Principal**: Make a request for an action or operation on an AWS resource. Users, roles, federated users, and applications are all AWS principals.
* **Request**: The principal sends a request to AWS when  tries to use the AWS Management Console, the AWS API, or the AWS CLI, 
* **Authentication**: As a principal, you must be authenticated (signed in to AWS) to send a request to AWS.
* **Authorization**: During authorization, AWS uses values from the request context to check for policies that apply to the request. It then uses the policies to determine whether to allow or deny the request.
* **Actions or Operations**: Things that you can do to a resource, such as viewing, creating, editing, and deleting that resource. 
* **Resources**: A resource is an object that exists within a service. 

### Overview of Identity Management: Users

#### First-Time Access Only: Your Root User Credentials
When you create an AWS account (with password and email), you create an AWS account root user identity. This combination of your email address and password is also called your **root user credentials**.
#### IAM Users
You can create individual IAM users within your account that correspond to users in your organization. IAM users are not separate accounts.
#### Federating Existing Users
If the users in your organization already have a way to be authenticated, such as by signing in to your corporate network, you don't have to create separate IAM users for them. Instead, you can federate those user identities into AWS.
Federation is particularly useful in these cases:
* Your users already have identities in a corporate directory.
* Your users already have Internet identities.

### Overview of Access Management: Permissions and Policies
Access management define what a user or other entity is allowed to do in an account. This process is often referred to as authorization.

#### Policies and Accounts
If you manage a single account in AWS, then you define the permissions within that account using policies.
#### Policies and Users
ou give permissions to a user by creating an identity-based policy, which is a policy that is attached to the user
#### Policies and Groups
You can organize IAM users into IAM groups and attach a policy to a group. 
#### Federated Users 
Federated users don't have permanent identities in your AWS account the way that IAM users do. To assign permissions to federated users, you can create an entity referred to as a role and define permissions for the role. 
#### Identity-based and Resource-based Policies
 *Identity-based policies*  are permissions policies that you attach to a principal (or identity), such as an IAM user, group, or role. 
* **Managed policies** – Standalone identity-based policies that you can attach to multiple users, groups, and roles in your AWS account. Two types:

    * AWS managed policies – Managed policies that are created and managed by AWS. 

    * Customer managed policies – Managed policies that you create and manage in your AWS account.

* **Inline policies** – Policies that you create and manage and that are embedded directly into a single user, group, or role.

*Resource-based policies* control what actions a specified principal can perform on that resource and under what conditions. Resource-based policies are inline policies, and there are no managed resource-based policies.

*Trust policies* are resource-based policies that are attached to a role.  They define which principals can assume the role.

### Security Features Outside of IAM
Some AWS products have other ways to secure their resources
* Amazon EC2: You log into an instance with a key pair (for Linux instances) or using a user name and password (for Microsoft Windows instances).
* Amazon RDS: You log into the database engine with a user name and password that are tied to that database.
* Amazon EC2 and Amazon RDS: You use security groups to control traffic to an instance or database.
* Amazon WorkSpaces: Users sign in to a desktop with a user name and password.
* Amazon WorkDocs: Users get access to shared documents by signing in with a user name and password.

### IAM Best Practices and Use Cases
 * Lock Away Your AWS Account Root User Access Keys
 * Create Individual IAM Users
 * Use Groups to Assign Permissions to IAM Users
 * Use AWS Defined Policies to Assign Permissions Whenever Possible
 * Grant Least Privilege
 * Use Access Levels to Review IAM Permissions
 * Configure a Strong Password Policy for Your Users
 * Enable MFA for Privileged Users
 * Use Roles for Applications That Run on Amazon EC2 Instances
 * Use Roles to Delegate Permissions
 * Do Not Share Access Keys
 * Rotate Credentials Regularly
 * Remove Unnecessary Credentials
 * Use Policy Conditions for Extra Security: restrict IP address, enable MFA
 * Monitor Activity in Your AWS Account

### Identities (Users, Groups, and Roles)
* The AWS Account Root User
* IAM Users
* IAM Groups
* IAM Roles 
* Temporary Credentials

### Warnings
* If a request to change some data is successful, the change is committed and safely stored. However, the change must be replicated across IAM, which can take some time. Such changes include creating or updating users, groups, roles, or policies. We recommend that you do not include such IAM changes in the critical, high-availability code paths of your application. 
* Policies and Users: Actions or resources that are not explicitly allowed are denied by default.
* You cannot attach a resource-based policy to an IAM identity.
* Do not use your AWS account root user access key 
* IAM Group are not truly an identity because it cannot be identified as a Principal in a resource-based or trust policy. 
* IAM Roles does not have any credentials (password or access keys) associated with it.

### Limits
* Up to two access keys per IAM user 
