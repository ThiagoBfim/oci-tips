# Oracle Cloud Infrastructure Developer 2021 Associate (1Z0-1084-21)

## Cloud Native

- Describes container-based environments, deployed as microservices and managed on elastic infrastructure through agile DevOps, continuous delivery workflows.

- Build, run, and improve apps based on well-known techniques and technologies for cloud computing.


## Cloud Native Environment / Building Blocks

- Microservices = Architectural style
  - Smaller code bases
  - Managed by independent teams
  - Independently deployable
  - Communication through APIs
  - Own tests, builds, data, deployments

- Functions & Containers = technologies serving a particular purpose.


## API Management

### API Gateway

The API Gateway provide an interface for client and the backend.

The API Gateway provides security, mediation and monitoring.

VCN must exists before creating an API gateway.

API Gateway instances can run on separate Availability Domain or Fault Domain.

Having deployed the authorizer function, you enable authentication and authorization for an API deployment by including two different kinds of request policy in the API deployment specification:
* An authentication request policy for the entire API deployment that specifies:The OCID of the authorizer function that you deployed to Oracle Functions that will perform authentication and authorization.The request attributes to pass to the authorizer function.Whether unauthenticated callers can access routes in the API deployment.
* An authorization request policy for each route that specifies the operations a caller is allowed to perform, based on the caller's access scopes as returned by the authorizer function.

### Polices

Users are add to a Group of Polices.

Group of Polices management:

- Network Resources
- API Gateways
- Functions
- Logging

It's necessary to add a Ingress Rule to enable Gateway API connection.

Polices are use to form Oracle Cloud Infrastructure Identity and Access Management to govern resources in a tenancy.

### Create an API

It's easy to create a new API with OCI.

1. Create a swagger.yaml with all specification
2. Import this swagger in the option of new API
3. Configure the Gateway.
4. Define a name and the path prefix
5. Add Security
 * Allowed Origins
 * Methods
 * Allowed Headers

 It's possible to Configure the Rate limiting, it's possible to limit the numbers of request per second.

 The Oracle provide integration with JWT authentication, or others like Basic authentication using Function.

#### Transformations

Oracle provides different types of transformations.

With Transformations it's possible to change the headers result that it's provide to the response.

#### Monitoring and Logging

OCI provide a Logging and dashboard to monitoring the API call.

### OCIR - OCI Container Registry

The OCIR provides low-latency to download the images, that can be used with Container Engine for Kubernetes, Functions, or for Compute

Requires a OCI Auth Token.\
Set up IAM policy grants to view, pull, push and manage the repository in the tenancy.

- Oracle Management
- Docker Images
- Standards-Based (ARM, x64)
- IAM Integration

#### Docker Images

- Create the dockerfile local.
- Create the image local.
- Login in the OCI registry
- Push the image to the registry

The push command can be execute with:

- Docker CLI
- Docker v2 API
- Oracle Functions Service

**Best Practices:**
- Network-close deployment (low-cost and low-latency)
- Create repositories in compartment (Share with different teams)
- Deleting unused image data
- Limit public repositories

OBS: Deleted images can be undeleted up to 48 hours later. They only can be restored using the CLI or API.

It's possible to untagging the image too.

#### Retention policy

Its possible to create automatic deletion criteria options:
- Remove after N days without pull request.
- Have not been tagged for a number of days.
- Have not been given particular exemption Docker tags


In each region in a tenancy, there's a global image retention policy. The global image retention policy's default selection criteria retain all images so that no images are automatically deleted. However, you can change the global image retention policy so that images are deleted if they meet the criteria you specify. A region's global image retention policy applies to all repositories in the region, unless it is explicitly overridden by one or more custom image retention policies.

You can set up custom image retention policies to override the global image retention policy with different criteria for specific repositories in a region. Having created a custom image retention policy, you apply the custom retention policy to a repository by adding the repository to the policy. The global image retention policy no longer applies to repositories that you add to a custom retention policy.

#### Scanning images for vulnerabilities

It's possible to include the scan for security vulnerabilities published in the CVE database.

Just add an image scanner to one or more repositories.\
The scan result are retained for 13 months.\
Scans can be automated using CI/CD tools.

#### Signing images for security

It's possible to use a master encryption key stored in OCI Vault for ensure that the image that will be pull have the correct signature.

That can be done using the OCI CLI or the REST API.

Images signatures can be managed in the OCI.

## OKE - Oracle Container Engine for Kubernetes

There are two parts:

1. Control plane Nodes, that is managed by Oracle

- Controller Manager
- Kube-apiserver
- Kube-scheduler
- Cloud Controller Manager
- etcd

2. **Worker Nodes**, that is managed by Customer

- Contains the Node Pool and the Nodes.


To access the Node it's possible using ssh into the nodes using private key.
### Worker Nodes

Works Nodes supports different types of shapes.

- Flexible shapes
- Bare Metal Shapes
- Standard & GPU Shapes
- HPC Shapes
- Dense I/O Shapes

#### Operation Systems

- When new versions are added the oldest is supported for at least 30 more days.
- Oracle keep three versions supported at all times for new clusters.

#### Subnet Configuration

It's a recommendation to create the :

- Kubernetes API
- LoadBalancer
- Worker Nodes

Each one in one different subnet network


### Managed Clusters & Node Pools

#### Clusters:

- Toggling the Pod Security Policies
- Changing the visibility of the API Endpoints
- Using NSGs to control traffic
- Clusters can be upgrade once they are created. A cluster upgrade upgrades the control plane nodes.

#### Node Pools:

- Node Pools properties can be updated
- Updated the properties are only applied to new nodes in the pool
- Always use Node Pool scaling to manage node count. Node count in a pool can be 0.
- Remove node will only remove the node from the management of Kubernetes, and not update the node pool or remove the underlying instance.


#### Upgrading Clusters

Upgrading Control Plane Nodes:
- Zero Downtime
- Explicitly invoked though the console or APIs
- No Downgrades
- Follows Kubernetes version skew policy

Upgrading Worker Nodes:
- In Place: Update the version in a node pool and re-creates the workers
- Out-of-place: Creates a new node pool and retires the old one.

### Scale

#### Workloads

It's possible to scale the pods with *Horizontal Pod Autoscaling**, with that will be created more pods to deal with request.

Another way is use **Vertical Pod Autoscaling**,  with that the pod will use more resources if needed.

#### Infrastructure

This is used to increase the Nodes or Node Pools. With that it's possible to increment the memory and CPU for workloads use.


### Security & Cluster Access

- Using CLI like Kubectl
- Using Access Token
- Service Accounts
- SSH Access

Obs: Kubernetes does not encrypt secrets by default

It's possible to use Vault Service to encrypt the value. OCKE can encrypt all secrets using OCI Vault. It's need to be set up at cluster creation.
**Be careful: Loss of the master encryption key results in permanent data loss**

#### Admission Controllers

Evaluate the request to the api-server after authn/authz.

The entire request is rejected if a single admission controller rejects it.

The admission controllers available in OKE are managed by Oracle.

Supported admission controllers change with each version of Kubernetes.

### Pod Security Policies

1. Create the Pod Security Policy Object
2. Create a (cluster) role that grants access to the Pod Security Policy.
3. Bind the (cluster) role to a user/service account using a (cluster) rolebinding.
4. Update the cluster to enforce the Pod Security Policy admission controller.

## Functions

Functions is Polyglot Containers. Functions packaged as containers and support different programming languages.

- Function-as-a-service
- Oracle Cloud Integrated
- Container Native
- Open Source
- Secure
- Pay per use
- Autonomous (scale)
- Event-Driven (Triggers to run)

To enable a function to access another Oracle Cloud Infrastructure resource, you have to include the function in a dynamic group, and then create a policy to grant the dynamic group access to that resource.

### How does it work?

1. Push Docker Image that is the function
2. Configure the function trigger (Exemple: HTTP Request)
3. Code runs only when triggered
4. Pay only for execution time


### Functions Pricing

Requests :
- Free 2 million requests/month
- $0.2 per million thereafter

Resource Usage:
- Free 400.000 GB-seconds per month
- $0.00001417 per GB-second thereafter

### Trigger

- Events Services

Real-time, fast alerts on state change

- Notifications

Multi-tenant, pub-sub pattern.\
Integrated with OCI Services (Events, Monitoring)\
Secure, highly reliable, low latency, durable.

- Service Connectors (Log, Data Streams)

Provide to take real-time actions.\
Easily emit log-based metrics.\
Integrate with 3 party tools with Kafka-compatible OCI Streaming

- API Gateway

Serverless platform, that allows to create RESTful, Serverless APIs backed by Oracle Functions.\
Authorization is possible with IDCS(Identity Cloud Service) or any other provide compatible of generation JSON JWT.

- Oracle integration
- CLI/SDK

### Invoke Function

You can invoke a function that you've deployed to Oracle Functions from:
- The Fn Project CLI.
- The Oracle Cloud Infrastructure SDKs.
- Signed HTTP requests to the function's invoke endpoint. Every function has an invoke endpoint.
- Other Oracle Cloud services (for example, triggered by an event in the Events service) or from external services.
so You can then deploy your code, call it directly or trigger it in response to events, and get billed only for the resources consumed during the execution.
Below are the oracle services that can trigger Oracle functions
-Events Service
-Notification Service
-API Gateway Service
-Oracle Integration service(using OCI Signature Version 1 security policy) so OCI Registry services cannot trigger your functions directly



### Uses Cases

Trigger code in Response to Events.

- Event-Driven Governance (Based on Logs)
- Web, Mobile API Backends (API Gateway)
- Real-Time file, Stream Processing (Take an action when data comes into Data Bucket Storage or Kafka Stream)
- Machine Learning, DevOps


### Concepts

#### Application:

- Collection of functions
- Attached to customer's VCN used for egress
- Unit of workload isolation
- Enable/disable logs, traces and set common environment variables

#### Function:

- Container image stored in a container registry

Configuration:
- Memory
- Timeout
- Environment variables(config)
- Service limits (number of functions, number of applications or concurrent calls)

Observability:
- Metrics
- Logs
- Traces
- Provide a Troubleshooting guide.


#### FN CLI

- Commands to manage functions
- Generates boilerplate to different programming languages.


## Streaming

### Distributed Streaming

- Point-to-Multipoint
- Real-time
- Ex: Apache Kafka

Provides End-to-End Encryption with TLS, but is possible to use OCI Vault too.

Fully integrated with OCI IAM to verify who can produce and consume the streaming.

It's possible to use VCN to stay in private communication.

Availability SLA = 99.95%

Stream Archiving can be configured using a Service connector to a OCI Object Storage Bucket.

#### Use Cases

- Integrate with Functions before send the data for Object Storage.
- OCI Service Connector Hub (Example Logs) - Kafka - Notification
- Its possible to connect with Third-Part connectors.


#### Concepts

- Message: Base64
- Key: Identifier to group related messages
- Stream: Append-only log of messages
- Stream Pool: Grouping of streams for management
- Partition: Section of a Stream
- Offset: Message location
- Cursor: Pointer to location


### Publish/Subscribe

- Point-to-Multipoint
- Push-based aync semantics
- Ex: Oracle JMS

### Message Queue

- Point-to-point
- Reliable async communication
- EX: RabbitMQ, ActiveMQ

### Request/Response

- Point-to-point
- Synchronous communication
- Ex: HTTP, gRRPC

## Events

**Real-time, fast alert on state change!**

OCI Events Service provide Automation based on state changes(OCI Services, Lifecycle Changes, System Events or Resource CRUD).

Example: Send Notification after the DB backup is done.


**Event:** Structured and schematized messages that denote a change in a resource\
**Rule:** The configuration where a user defines events that need to be created, and triggers an action if they occur.\
**Action:** The user-defined response to when an event occurs. For example, triggering a function or writing to a stream.

The Event Envelop is a JSON that contains the EventType, the source, the eventTime, and others.

The Event Payload contains the CompartmentID, ResourceName, freeFromTags, definedTags, and others.

There is Over 60 OCI Services and 1000s of Event Types

### Rules

Rules are OCI objects that allow you to select:
- Which event types to monitor
- Which specific resources to filter
- Which actions should be triggered

#### Typical Rule Design Workflow:

1. Identify Action Resources
2. Plan Filtering (Event Types and pattern matching strategy)
3. Create the rule

#### Creating Rules

1. Choose the Name and Compartment
2. Create the Trigger condition. Example: "Event Type = Delete bucket"
3. Response Action. Example: "Notify DRI (ONS)"

#### Rule Design Considerations

* Each Rule is scoped to a single compartment or compartment hierarchy
* All rules and actions are validated for proper IAM permissions.
* Consider creating rule conditions by leveraging filter tags.
* The OCI tenancy is limited to 50 rules. Rules can include multiple conditions
* Events are guaranteed to be evaluated at least once.
* At least one delivery attempt will be made for all actions.
* Delivery retry will take place for up to 5 hours or until a non-retryable error occurs.
* Rules Metrics is persisted for 90 days

#### Uses Cases

- Perform cleanup tasks when an OCI resource is terminated
- Analyze network logs when anomalies are detected in the network.
- Process files when they are uploaded in an Object Storage bucket.
- Archive events to a stream for later analysis.
- Trigger an action when a new resource is created or deleted in a compartment.


## Security

### IAM - Identity and Access Management

- AuthN - Who are you?
- AuthZ -  What permissions do you have?
- Auditing - What did you do?

Components:

- Dynamic Groups
- Users
- Roles

#### AuthN

**Principals**
 - IAM Users
 - Group Users
 - Resource Principal

**Authentication**
 - User name, password
 - API Signing Key (RSA key pair)
 - Auth tokens (Third party APIs)

#### AuthZ

 Allow [SUBJECT] to [ACTION] in [PLACEMENT] where [CONDITION]

 AuthZ is about permissions.

**Subject:** Subjects are a clause for the various ways that an authenticated actor can be addressed.
- By membership/Group
- By service
- As a wildcard "any-user"

A subject can use more than one subject element. Example: Group alice, Bob

 **Actions:** Services define one or more permissions that any given API call will require.
 - Inspect
 - Read
 - Use [Permissions to modify pre-existing resources]
 - Manage [Do anything to the resource kind]

 The Action can be set to just one Individual resource type, or to an Aggregate resource-type.


### Compartment

Create compartment to have isolate and control access.

Example: compartment Network, Compartment Storage.

It's possible to move resource from compartments.

The resource belongs to only one compartment.

It's possible to have nested compartments, 6 it's the maximum size.

Set quota and budgets for each compartment.

It's possible to have one compartment in multiple regions.

Provides logical isolation for resources

### Vault

OCI Vault: Centrally Managed Keys and Credentials

**Envelope Encryption:** There is a master key, and a key for each environment.
For example: There is a master key and a Data key for Block Storage, this provide more security and it's easier to manage.

Vault provides two types of encryption keys: Data encryption keys and master encryption keys.

The master key can't be recovered after delete. But there is 7-day gap to recover the master key.

Vaults are logical entities where Key Management create and durably stores your keys

Keys are logical entities referencing one or more key versions that contain the cryptographic material you use to protect your data


There is two options:

1. Software

With Software, the secret is stored in Server, and can be exported from server. Operations allowed on Clients.

2. Hardware Security Modules(HSM)

Stored in HSM Device, and the secret can't be exported from HSM.

Oracle Cloud Infrastructure Vault is a managed service that lets you centrally manage the encryption keys that protect your data and the secret credentials that you use to securely access resources.

Supports encryption: ECDSA(Elliptic curve digital signature algorithm), AES(Advanced Encryption Standard), RSA(Rivest-Shamir-Adleman)

#### Key Management - Design Considerations

* Regional service
* Replicates encryption keys across 3ADs in multi-AD regions & within a single AD.
* Services
 - Block Volume, Object Storage, File Systems, OKE, Streaming are integrated with Key Management
 - Autonomous Container Databases on dedicated Autonomous Exadata Infrastructure and Exadata Databases (without Oracle Data Guard enabled)
* Key Rotation
 - Rotation a master key does not impact on the data already encrypted.
 - Customers can continue to decrypt data using an older master key version.
 - Any subsequent encrypt/decrypt after the master key rotation would use the lastest version of the master key.
* Its possible to force re-encryption of all data and delete the prior key version.
* You can schedule the deletion of a vault by configuring a waiting period for deletion from 7-30 days. (after delete it can't be recovered)

### Web Application Firewall

Oracle Provides a lot of features for WAF in layer 7.

By Default there are a lot of protection, like SQL Injection, XSS and others.

Examples:
- Show Capcha accordingly the Region of the request
- Block the request accordingly region, IP, or other rule.

### IAM Policy and Dynamic group

Dynamic groups allow you to group Oracle Cloud Infrastructure compute instances as "principal" actors (similar to user groups)

Create Dynamic group and add a policy to make API calls against other OCI Services from your instance without configuring user credentials.

When creating a dynamic group, you must provide a unique, unchangeable name for the dynamic group. The name must be unique across all groups within your tenancy.



## Observability and Management

- Integrated Platform
- Multi Cloud, on-premises
- Cross-tier View
- Open Standards

### Monitoring

With Metrics, for example: CPU, Memory, it's possible to create alarms, for example, if CPU utilization is more than 80% will trigger the alarm.

Its possible to create a lot of types of Metrics.

The Alarm is responsible to alert the responsible users, send email, SMS, or other message.

Alarms use metrics for monitoring and consists of a trigger action and notification method.

The Oracle Cloud Infrastructure Monitoring service enables you to actively and passively monitor your cloud resources using the Metrics and Alarms features.

### Logging

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

### Operations Insight

Operations Insights Capacity Planning provides deep insights into the allocation and usage of CPU, storage, memory and I/O resources by databases within:

- OCI compartment
- Databases monitored by oracle Enterprise Manager Cloud Control
- Hosts and databases monitored by an OCI Management Agent service

### Application Performance Monitoring

- Rapid Troubleshooting
- Insight
- DevOps collaboration

Features:

- Distributed tracing service
 - Monitoring and alerting
 - Diagnostics
 - Exploration and analysis
- End-user monitoring
 - Browser instrumentation
 - Session Diagnostics
 - Combined with server side tracking
- Synthetic monitoring
 - Browser and Scripted Browser monitors
 - REST API and scripted REST monitors
 - Combined with server-side tracing
 - OCI vantage points (multiple regions)
- Server monitoring
 - Open metrics/open telemetry compatibility
 - AppServer metric collection (Collect metrics from JVMs)
 - Integration with OCI Monitoring and Logging Analytics services


----------
## Quiz

1. An Oracle Function is developed to generates static pages during the function call. What should be used to provide a way that every regional URL will hit the same application endpoint?

R: Create a deployment and add Path Parameters & Wildcards to Route Paths.

2. Maximum Timeout of Oracle Funcion

R: 5 minutes

3. Maximum memory threshold  for a Oracle Function

R: 1024 MB

4. What is the best OCI service to provide an Web appliation based on REST?

R: OCI API Gateway

5. Witch services is supported using OCI Service Broker for Kubernetes

R: Object Storage\
Autonomous Transaction Processing\
Autonomous Data Warehouse\
OCI Streaming Service

**OCI Events Service is not supported**

6. Your application team wants to use configuration variable for their Application Pods and wants to inject it before the Pod creation.

R: Use PodPresent to create the config outside of Pod

7. What is Rolling Update?

R: Rolling updates allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones

8. You need to make sure that the deployment application maintains the desired replica state at all times while updating the application with a new image.

R: Apply maxSurge and maxUnavailable in the Deployment spec

9. You weant to allow applications running on an OCI compute instance leveraging OCI SDKs to call other OCI services.

R: Instance Principal is the capability in the Oracle Cloud Infrastructure Identity and Access Management (IAM) service that allows you to make service calls from an instance

10. Supported SDK in OCI

R: SDK for Java\
SDK for Python\
SDK for TypeScript and JavaScript\
SDK for .NET\
SDK for Go\
SDK for Ruby

11. Which is the different ways to get authenticated using OCI SDK?

R: Using Security Token\
Using resource Principal\
Using instance Principal\
Using OCI CLI Config file

12. How update the deployment of Infrastructure as Code ?

R: remote-exec provisioner helps invoke a script on the remote resource once it is created.

13. Which two are required to enable Oracle Cloud Infrastructure (OCI) Container Engine for Kubernetes (OKE) cluster access from the kubectl CLI?

R:  Install and configure the OCI CLI\
A configured OCI API signing key pair

14. What is the default Load balancer shape used in OCI container Enginner for Kubernetes?

R: 100 Mbps

15. You are building a cloud native, serverless travel application with multiple Oracle Functions in Java, Python and Node.js. You need to build and deploy these functions to a single applications named travel-app.

R: fn deploy --ap travel-ap -- all

16. A service you are deploying to Oracle infrastructure (OCI) Container En9ine for Kubernetes (OKE) uses a docker image from a private repository Which configuration is necessary to provide access to this repository from OKE?

R: Create a docker-registry secret for OCIR with identity Auth Token on the cluster, and specify the image pull secret property in the application deployment manifest.

17. Given a service deployed on Oracle Cloud infrastructure Container Engine for Kubernetes (OKE), which annotation should you add in the sample manifest file to specify a 400 Mbps load balancer?

R: service . beta. kubernetes . lo/oci-load-balancer-**shape**: 400Mbps

18. Which two statements accurately describe Oracle SQL Developer Web on Oracle Cloud Infrastructure (OCI) Autonomous Database?

R:
* It is available for databases with both dedicated and shared Exadata infrastructure.
* It provides a development environment and a data modeler interface for OCI Autonomous Databases.

19. Who is responsible for patching, upgrading and maintaining the worker nodes in Oracle Cloud Infrastructure Container Engine for Kubernetes (OKE)?

R: Customer

20. Who is responsible for patching, upgrading and maintaining the Control plane Nodes in Oracle Cloud Infrastructure Container Engine for Kubernetes (OKE)?

R: Oracle

21. What is the minimum amount of storage that a persistent volume claim can obtain In Oracle Cloud Infrastructure Container Engine for Kubemetes (OKE)?

R: 50 GB

22. What is the difference between blue/green and canary deployment strategies?

R:  In blue/green, both old and new applications are in production at the same time. In canary, application is deployed Incrementally to a select group of people.

23. Which concept is NOT related to Oracle Cloud Infrastructure Resource Manager

R: Queue

24. Which two statements accurately describe an Oracle Functions application

R:
* A common context to store configuration variables that are available to all functions in the application
* A logical group of functions

25. Which two statements are true for serverless computing and serverless architectures?

R:
* Application DevOps team is responsible for scaling
* Long running tasks are perfectly suited for serverless

26. Describes Oracle Cloud Infrastructure (OCI) Load Balancer integration with OCI Container Engine for Kubernetes (OKE)?

R: OKE service provisions a single OCI Load Balancer instance shared with all the Kubernetes services with LoadBalancer type in the YAML configuration.

27. What is the open source engine for Oracle Functions?

R: Fn Project

28. How invoke the OCI Function:

You can invoke a function that you've deployed to Oracle Functions in different ways:
* Using the Fn Project CLI.
* Using the Oracle Cloud Infrastructure CLI.
* Using the Oracle Cloud Infrastructure SDKs.
Making a signed HTTP request to the function's invoke endpoint. Every function has an invoke endpoint.

29. Which two are benefits of distributed systems?

R:
* Scalability
* Resiliency

30. You have deployed a Python function that uses the Oracle Cloud Infrastructure (OCI) Python SDK to stop any OC1 Compute instance that does not comply with your corporate security standards. But it's not working. How should you troubleshoot this?

R: Enable function logging in the OCI console, include some print statements in your function code and use logs to troubleshoot this.

31. Which statement is true with regards to service resiliency?

R: Resiliency is about recovering from failures without downtime or data loss.

Resiliency and availability refers to the ability of a system to continue operating, despite the failure or sub-optimal performance of some of its components.
https://docs.cloud.oracle.com/en-us/iaas/Content/Functions/Concepts/functionsavailability.htm

32. You want to push a new image in the Oracle Cloud Infrastructure (OCI) Registry. Which two actions do you need to perform?

R:
* Assign a tag via Docker CLI to the image.
* Generate an auth token to complete the authentication via Docker CLI.

33. Per CAP theorem, in which scenario do you NOT need to make any trade-off between the guarantees?

R: when there are no network partitions

34. What is a Service Aggregator Pattern?

R: It involves implementing a separate service that makes multiple calls to other backend services

## Links:

* https://learn.oracle.com/ols/exam/35644/99037/146398
* https://www.freecram.com/exam/1Z0-1084-20-oracle-cloud-infrastructure-developer-2020-associate-e11226.html
* https://www.examtopics.com/exams/oracle/1z0-1084-20/
* https://www.certification-questions.com/oracle-exam/1z0-1084-20-dumps.html
* https://github.com/kaustavk/Oracle-1Z0-1084-20
