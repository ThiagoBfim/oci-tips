# Oracle Cloud Infrastructure Foundations (1Z0-1085-21)

Book: https://learning.oracle.com/player/play?in_sessionid=18A4441AJ8AJ4302&classroom_id=216054728

The Oracle Cloud offers 7 things:

1. Core Infrastructure
 * Compute (CPUs), Bare metal
 * Containers, Kubernetes, Service Mesh, Registry
 * OS, Vmware
 * Storage
 * Networking, VPN, Cluster, Gateway
2. Data & AI
 * Big Data
 * AI Services
 * Messaging, Kafka, Streaming
3. Databases
 * Oracle Databases (JSON, ATP)
 * Distributed & OSS Databases (NoSQL, MySQL)
4. Developer Services
 * Low Code (Apex)
 * AppDev (SQL Developer, GraalVm)
 * Infrastructure as Code (Terraform, Ansible)
5. Applications
 * Serveless
 * App Integration (Email, Notifications)
 * Business & Industry SaaS
6. Analytics
 * Analytics Cloud
7. Governance & Administration
 * Cloud Ops
 * Security
 * Observability (Monitoring, Logging, Notifications)

## OCI Architecture

### Regions

Prefer regions:
* Close to the users to have lowest latency and highest performance.
* Be careful with data residency & compliance.

### Service Availability

* Availability Domains (AD)
* Fault Domains (FD)

With AD and FD it's possible to prevent the system to get down, because it will avoid to have just one point of failure.

There is one element called **Data Guard** to keep the database in different regions synchronized.

**Important Notes:**
* Every availability domain has three fault domains for high availability.
* A fault domain is a grouping of hardware and infrastructure within an availability domain. Each availability domain contains three fault domains for high availability.
* Oracle Cloud Infrastructure is hosted in regions and availability domains. A region is a localized geographic area.
*  Fault domains provide anti-affinity: they let you distribute your instances so that the instances are not on the same physical hardware within a single availability domain. A hardware failure that affects one fault domain does not affect instances in other fault domains.

### Tenancy/Root Compartment

The Admin user (Tenanct Admin) is into Administrations Groups to manage the Polices.

**Best Practice:** Create dedicated compartments to isolate resources.

Example:

Tenancy Admin -> Service Admins -> Groups (Storage-Admin, Network-Admin)

**Best Practice:** Don't use Tenancy Admin account for day-to-day operations.

**Best Practice:** Use Multi-Factor Authentication (MFA)

## Identity and Access Management (IAM)

Fine-grained Access Control

AuthN - Who are you?\
AuthZ - What permissions do you have?

The OCI Resource is recognized by Oracle Cloud ID (OCID)

Example: ocid1.<RESOURCE TYPE\>.<REALM\>.[REGION].[FUTURE.USE].<UNIQUE ID\>

#### AuthZ

Permissions, with IAM Polices

Polices: Human readable statements to define granular permission

Allow <group_name\> to <verb\> <resource-type\> in <location\> where <conditions\>

Verbs:
- Manage  (Administrator)
- Use     (Resources and Users)
- Read    (Internal auditors)
- Inspect (Third-party auditors)

Resource Type:
- all-resources
- database
- instance
- object
- virtual-Network
- volume family

#### Compartment

Create compartment to have isolate and control access.

Example: compartment Network, Compartment Storage.

It's possible to move resource from compartments.

It's possible to have nested compartments, 6 it's the maximum size.

Set quota and budgets for each compartment.

It's possible to have one compartment in multiple regions.

Provides logical isolation for resources

#### AuthN

Which is  a valid method for authenticating a Principal in OCI Identity and Access Management (IAM) service?

* API Signing Key
* Console user name, Password
* Auth Tokens
* INVALID -> OCI Vault Master Encryption Keys (*)

## Networking

### VCN

Virtual Cloud Network(VCN) provides a private network with secure communication, high availability and scalable.

Which components are created by default with the creation of a virtual cloud network (VCN)?
* Default set of DHCP options, with default values
* Default security list, with default security rules
* Default set of route tables, with no rules.

Each subnet in a VCN can exist in a single availability domain or across an entire region.

The VCN have two firewall features that can be used to control traffic:
1. Network Security Groups
2. Security Lists

A subnet is a subdivision of a VCN. For an overview of VCNs, allowed size, default VCN components, and scenarios for using a VCN

#### Internet Gateway

Allows users in internet to access the services.

An internet gateway allows both inbound and outbound traffic.

#### NAT Gateway

It's possible to add an NAT Gateway to enabled the services to access the internet, but block the service to be accessed by internet.

Blocks inbound traffic, but enables outbound traffic to the internet.

#### Service Gateway

It's a private Gateway to provide communication inside the services, without using internet. Example: Application and the database.


#### Dynamic Routing Gateway/On Premisse

Allow use with internal VPN.\
Providing a path for traffic between your on-premises networks and VCNs, and can also be used to route traffic between VCNs.

#### Peering

Use to communication between internal network.

Remote Peering is used to communication from different regions.

Peered VCNs can exist in different tenancies.

#### Security

It's possible to create:
* Security List
* Network Security Group

### Load Balancer

It distributes traffic to different backend servers in a virtual cloud network (VCN).

#### LoadBalancer Type 1:

Operates in Layer 7 Http/s

More intelligent to deal with HTTP Packages

#### Networking LoadBalancer Type 2:

Operates in Layer 4 TCP/UDP

More Faster and with lower latency.


## Compute

* Virtual Machine: Is a Machine shared with others customers. Is isolated and safety.
* Bare Metal: Is a physical machine.
* Dedicated Host: Is a Machine Dedicated to a unique customer.

**Once allocated, the primary private IP for the instance is not editable.**

Supports Processors:
* Intel
* AMD
* ARM-Based

The ARM-Based have better price processor value.


### Scaling

#### Vertical Scaling

This is used to scale just one machine, so there are some problems:

* It's requires downtime
* New shape must be same hardware architecture

#### Horizontal/Autoscaling Scaling

It's possible to scale down and scale up, without downtime.
If one machine fail, the others keep working.

To configure Autoscaling is necessary to configure some things:

1. Instance Pool before scale.

Example: Minimum Size = 1, maximum Size = 5.

2. Scaling Rule

Example: If CPU or Memory > 70% add 2 instances.

### OS Management Service

OS management can be used for automating patches, simplifying package management and managing CVE (Common Vulnerability Exposure)

* Automated Patch Management
* Simplified Package Management
* Common Vulnerabilities and Exposures Lookup

## Storage

#### Local NVMe

- Locally attached storage.
- High performance.

#### Block Volume

- Persistence in another server
- Provide more persistent durability.
- Have lower performance, because it is in another server, so there will be the networking traffic.
- Online Resize
- Replication of Block Volumes

It can be attached to a compute instance.

Block Volume can't decrease the size of a block volume.


#### Type Tiers:

* Basic: Used with large sequential I/O (2 IOPS/GB)
* Balance: Balanced choice for random I/O (60 IOPS/GB)
* Higher Performance: Most I/O-demanding Workloads (75 IOPS/GB)
* Ultra Higher Performance: highest I/O-demanding Workloads (90-225 IOPS/GB)

There is one element called Disk that is READ/WRITE shareable, so multiple VMs can write and read this volume.



#### File Storage

Shared file system, is more common to use for shared from different applications, and it works like Block Volume, but with Files and Directories type.

File Storage is like the OS Storage, organized into hierarchical files.

Linus -> NFS
Windows -> SMB

**Uses cases:**
* Oracle EBS
* To storage and management the state of the microservices
* Analytics

There is data protection: Snapshots
There is Security: Data-at-rest and in-transit Encryption


#### Object Storage

This Storage is more common to be used to public access to internet, but it can be private too.

Example of used to be accessed by internet: logs, photos, PDF files.

The Object Store has 3 important things:

1. Object
2. Bucket
3. Namespace

Example: https://objectstorage.us-sanjose-1.oraclecloud.com/n/NAMESPACE/b/BUCKET/o/OBJECT

#### Type Tiers:

* Standard Storage Tier: Used for fast and immediate access. Can't be downgraded.
* Infrequent Access Storage Tier: Storage cost less, and should be used with data that you access infrequently. Minimum retention requirement for Infrequent access is 31 days.
* Archive Tier: Seldom or Rarely used. Minimum retention requirement for Infrequent access is 90 days. The Object needs to be restored before download.
* Auto Tier: Intelligent logic to move to the best Tier accordingly the access.

It's also possible to include the version management, so with that the object will be versioned by any update.

#### Archive Storage

This Storage is used for backup, for durability data.

The Archive Storage service is ideal for storing data that is seldom accessed, but requires long retention periods.

#### Persistence vs Durability

**Persistence:** Save the data
**Durability:** Copy the data to be sure that if the data is lost, there will be a copy.

#### Migration Services

- Support Offline migration with Data Transfer Disk
- Support Online Migration with Data Transfer Appliance

Storage Gateway Service.

The final User send request to Application.\
The Application communicate using NFSv4 with Storage Gateway\
The Storage Gateway provide the Resource with OCI Object or Archive Storage by HTTPS.


#### Lifecycle Management

There are ways to provide a lifecycle to the object, for example, after 120 days, the object have to be deleted.


## Database

* Virtual Machine (Fast Provisioning)
* Bare Metal (Fast Performance)
* RAC (Managed High Availability)\
Two-node Oracle RAC DB systems require Oracle Enterprise Edition - Extreme Performance.

* Exadata DB Systems (Managed Exadata Infrastructure)
* Autonomous Shared/Dedicated  (ADW/ATP) - Self-driving, self-securing and self-repairing

### Autonomous DB

An Autonomous Database handles the creating of the database along with backups, patching, upgrading, and tuning of the database.

Use ML to automate:

* Database Tunning
* Scale Up and Scale Out
* Security
* Backups
* Updates

2 Types:

1. **Shared**\
Oracle handles with Exadata infrastructure and management. User have to manage only the Autonomous DB.
2. **Dedicated**\
You have exclusive use of the Exadata hardware.

Dedicated deployment is a deployment choice that enables you to provision autonomous databases into their own dedicated cloud infrastructure.

ATP is a workload type on the Autonomous Database and not a DB system

### MYSQL in Cloud

**High Availability:** fault-tolerant system with automatic failover and zero data loss

**HeatWave:** Easily run high performance analytics against your MYSQL database, no ETL required\
Used with OLTP and OLAP Applications.\
400x faster than MySQL.

### Oracle NoSQL

NoSQL Database gives a predictable single-digit, millisecond latency with high performance workloads.

* Fully Managed
* Elastic
* High Performance
* Data model flexibility
* Access Control
* Low operating cost
* Developer friendly
* Always available
* Hybrid cloud

## Security Introduction

With Oracle Cloud Infrastructure, the Oracle manages:

* Virtualization
* Physical Hosts
* Physical Network
* Physical Datacenter

The user have to manage:

* Data
* Devices
* Accounts & Identifies
* Applications
* Network Controls
* Operating System


### Cloud Guard

**Targets**: Resources to be examined
**Detectors**: Identity issues
**Problems**: Potential security issue
**Responders**: Corrective actions

Example: Public Bucket is a target.
Create a rule that the bucket should no be public, so the detector will alert.
If the bucket is public in this moment, there will be a problem.
There is responders that can automatic responde to the problem with OCI Functions, or with OCI Notifications.


 A security zone is associated with a compartment and a security zone recipe.

### Vulnerability Scanning

Designed to check for vulnerabilities and deviations.

Verify Daily or Weekly

* Port Scanning: check the opening ports
* Agent Based Scanning: Check against standard benchmarks

### Vault

OCI Vault: Centrally Managed Keys and Credentials

**Envelope Encryption:** There is a master key, and a key for each environment.
For example: There is a master key and a Data key for Block Storage, this provide more security and it's easier to manage.

Vault provides two types of encryption keys: Data encryption keys and master encryption keys.

There is two options:

1. Software

With Software, the secret is stored in Server, and can be exported from server. Operations allowed on Clients.

2. Hardware Security Modules(HSM)

Stored in HSM Device, and the secret can't be exported from HSM.

Oracle Cloud Infrastructure Vault is a managed service that lets you centrally manage the encryption keys that protect your data and the secret credentials that you use to securely access resources.

Supports encryption: ECDSA(Elliptic curve digital signature algorithm), AES(Advanced Encryption Standard), RSA(Rivest-Shamir-Adleman)

### Web Application Firewall

Helps to filter traffic at Layer 7 (Application Layer)

Protect with:
* Cross site scripting Exploits (XSS)
* SQL Injections

Over 250 rule sets.

WAF can protect any internet facing endpoint, providing consistent rule enforcement across a customer's applications.

### Bastion

Provide restricted and time-limited secure access to resources that don't have public endpoints and require strict resource access controls.

Integrate with Identity and Access Management (IAM).

* Provides restricted access to target resources.
* Removes the need to create and maintain your own bastions.


## App Dev

The OCI services provides a compatibility with Terraform.

**Resource Manager:** The Resource Manager is managed with Terraform.
The Terraform file could be in a folder, bucket, git, or others places. And this file is sufficient to create the Stack, Plan, apply and provision.

Using Terraform, Resource Manager helps you install, configure, and manage resources through the "infrastructure-as-code" model.

### Function as-a-Service

The Cloud Provider is responsible for run the function without dealing with service.

* Event Driven Architecture
* Oracle Cloud Integrated
* Container Native
* Open Source
* Pay per use (Pay for execution, not for idle time)
* Autonomous (auto-scale)
* Event-Driven (Oracle Cloud infrastructure triggers to run your code)

The serverless and elastic architecture of Oracle Functions means there's no infrastructure administration or software administration for you to perform

### OKE - Oracle Container Engine for Kubernetes

The OKE possible to use the best of Kubernetes. \
Its possible to one Region has 3 fault Domain with some nodes and pods. \
This provides:

* Fully Managed
* Scalable
* Highly available

Oracle Cloud Infrastructure Container Engine for Kubernetes is a fully-managed, scalable, and highly available service that you can use to deploy your containerized applications to the cloud.

### OCIR - Oracle Cloud Infrastructure Repository

There is two types of OCIR:
1. OKE: This is like docker Repository, it's a place where the images will stay with private access
2. Functions: The OKE can be like a Github, a repository of code that can be public or private

A single registry can contain both private and public Docker repositories.

## API Gateway

The Gateway is the responsible to receive the high level user request, and send to the correct service. Nginx is one example of API Gateway.

The API Gateway enforces auth, SLA Metrics, alarms and logging, rate-limiting.

You can access the API Gateway service to define API gateways and API deployments using the Console and the REST API.

## Observability and Management

- Integrated Platform
- Multi Cloud, on-premises
- Cross-tier View
- Open Standards

### Monitoring

With Metrics, for example: CPU, Memory, it's possible to create alarms, for example, if CPU utilization is more than 80% will trigger the alarm.

Its possible to create a lot of types of Metrics.

The Alarm is responsible to alert the responsible users, sen email, SMS, or other message.

Alarms use metrics for monitoring and consists of a trigger action and notification method.

The Oracle Cloud Infrastructure Monitoring service enables you to actively and passively monitor your cloud resources using the Metrics and Alarms features.

## Logging

- Centralized Log Management
- Rule-based Actions
- Build on open standards

Types:
1. Audit Log
2. Service Log (OCI Service)
3. Custom Log (Application)

The OCI Logging service can be used to enable,manage and search critical diagnostic information that describes how resources are performing.

Be careful, logging is not used to metric, for that case is used Monitoring.

Log Analytics provides Index, enrich, and aggregate log data from applications, be careful, it's not logging service.

Log Service is a centralized single pane of glass for all logs in a Tenancy.

## Analytics and AI

1. Data Integration

- ETL Tasks (Extract, Transform, Load)

Create data integration Flow, to execute the ETL Tasks.

Data Integration enables the ETL developers to develop, build, and test data integration solutions.

2. Data Flow

Allows run Apache Spark. It's provide Big Data Processing.

Fully management, serverless Spark service with near-zero administrative overhead.

Data flow is used to easily create, share, run, and view the output of Apache Spark applications.

3. Data Catalog

- Self-Service Data Discovery
- Data Governance Solution
- Tools for Analytics, Data Science, Data Integration

Data Catalog can harvest technical metadata from a wide range of supported data sources that are accessible using public or private IPs.

Find the information you need by exploring the data assets, browsing the data catalog, or using the quick search bar.

4. Data Science

- Provide a collaborative UI for the Data Science, to build, create and management Machine Learning Models.
- Support OpenSource tools like JupyterLab, Keras, Tensorflow.

The Oracle Accelerated Data Science (ADS) SDK is a Python library that is included as part of the OCI Data Science service.

## Hybrid Cloud

### Self Contained:
- OCI Public Region
- OCI Dedicated Region
- OCI Roving Edge


### Remotely tetnered
- OCI Exadata Cloud@Customer
- To support applications that have stringent data privacy requirements
- To support applications that have extremely low latency requirements
- To support applications that have local data residency requirements

### Oracle Cloud VMware Solution

Oracle Cloud provide the different level of Controle, can have low or high control for:

- Customer responsibility
- Admin access
- Upgrade, patch, control
- Metadata Visibility

Oracle Cloud VMware solution is based on VMware ESXi and related technologies. It doesn’t support Hyper-V.

OCVS uses Bare Metal compute shape.

OCVS runs the certified VMWare product stack in the cloud.

### Dedicated Region

Is a dedicated Oracle Service that provides everything that Public Oracle provides.

All services, Operations, Availability SLA, Performance SLA, Manageability SLA, and other.

It allows you to build modern applications in your data center at the same predictable low pricing offered in Oracle's public cloud regions.

## Pricing

- Support as Pay as you Go(PAYG) in most of services
- Consumption Based Pricing, Charged per consumed resource. It's better than PAYG. This is used by functions
- Annual Universal Credits (Must use in 12 months)
- Bring your own License (BYOL)

Pricing depends on types of resources used.\
Budgets are set on cost-tracking tags or on compartments\
Ingress is free while egress rates are based on geography

 Oracle offers these billing models: Pay as you go, monthly universal credits, annual universal credits, and BYOL.

### Cost Management

- Support create Budgets, to alert if excess the expected cost.
- Cost Analytics
- Reports about Cost
- Limits, Quotas and Usage
- Compartment Quotas (Set maximum number of clud resources that can be used for a compartment)

**Budget:** Allows you to set up alerts to notify if a budget forecast is to be exceeded or spending surpasses a certain amount.

### Tagging

Key-Value Pair.\
Example:
- Environment = Production
- Project = Alpha

The Tags is useful to:
- Customize the organization of your resources.
- Cost management
- TAG based access control

There is two types:

1. Free-form Tags

It's simple, just a map of key and value.

2. Defined Tags

Contains more features and control.
- Contained in namescpaces.
- Defined schema, secured with Policy.

Example:

Namespace: Operations\
Tag: Operations.Environment="Production"

* Tag key definition or a tag namespace cannot be deleted, but retired.
* Tag Namespace is a container for a set of tag keys with tag keu definitions.
* Tag key definition specifies its key and what types of values are allowed.


## SLA

SLA - Service Level Agreement.

OCI SLA is defined as a number of nines for a month and a percentage credit.

The uptime percent is Identity by the % of unavailable service.

The Oracle give back one % of credit depending of the uptime %.

For example: If the vaults is less than 95.0% available, so Oracle give back 100% percent credit.

Mission-critical workloads also require consistent performance, and the ability to manage, monitor, and modify resources running in the cloud at any time. Only Oracle offers end-to-end SLAs covering performance, availability, and manageability of services.

Oracle offers end-to-end SLAs covering performance, availability, and manageability of services.

To log a service request, you need the customer support identifier, tenancy OCID, and Resource OCID.
