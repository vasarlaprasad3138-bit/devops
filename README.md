# devops
helping myself to track my progress



EC2 — Elastic Compute Cloud
Amazon Web Services EC2 gives you virtual machines/servers.

You choose things such as:

CPU and memory

Operating system

Storage

Network configuration

Security groups

Instance type

For example:

Internet
   ↓
Load Balancer
   ↓
EC2 Server
   ↓
Application
   ↓
Database
Think of EC2 as renting a computer in AWS's data center.

2. VPC — Virtual Private Cloud
A VPC is your private network inside AWS.

It contains things such as:

Subnets

Route tables

Internet Gateway

NAT Gateway

Security Groups

Network ACLs

A common architecture is:

                    Internet
                       |
                Load Balancer
                       |
              Public Subnet
                       |
             -------------------
             |                 |
        Private Subnet    Private Subnet
             |                 |
           EC2               EC2
             |
          Database
The important concept is that you can make some resources publicly accessible and keep others private.

3. EBS — Elastic Block Store
EBS provides persistent block storage, primarily for EC2.

Think:

EBS = hard drive/SSD attached to your virtual server.

For example:

EC2
 |
 +---- EBS 100 GB
 |
 +---- EBS 500 GB
If your EC2 instance is stopped, the EBS volume can persist.

EBS is commonly used for:

Operating system disks

Application files

Database storage

Persistent application data

4. S3 — Simple Storage Service
S3 is object storage, rather than a traditional disk.

It's commonly used for:

Images

Videos

Documents

Backups

Logs

Data lakes

Static website files

You organize objects into buckets.

S3 Bucket
│
├── images/
│   ├── image1.jpg
│   └── image2.jpg
│
├── documents/
│   └── report.pdf
│
└── backups/
    └── database.zip
A useful distinction:

EBS

Storage attached to a server.

S3

Storage accessed as objects through an API.

5. IAM — Identity and Access Management
IAM controls authentication and authorization in AWS.

For example:

Developer
   ↓
IAM
   ↓
Allowed → S3
Denied  → Production Database
IAM has concepts including:

Users

Groups

Roles

Policies

Permissions

A very important AWS concept is the IAM Role.

For example, instead of putting AWS credentials inside an application:

EC2
 ↓
IAM Role
 ↓
S3 permissions
The EC2 instance can obtain temporary credentials through its role.

6. CloudWatch
CloudWatch is AWS's monitoring and observability service.

It can collect:

Metrics

Logs

Events

Alarms

Dashboards

Example:

EC2 CPU
  ↓
CloudWatch
  ↓
CPU > 80%
  ↓
Alarm
  ↓
Notification
You can monitor things like:

EC2 CPU utilization

Lambda invocations

Application logs

API errors

Container metrics

7. Lambda
Lambda is serverless compute.

You upload your code and AWS runs it for you.

You don't manage:

Servers

Operating systems

VM patching

Server capacity

For example:

User uploads image
        ↓
       S3
        ↓
      Lambda
        ↓
Resize image
        ↓
Save result to S3
Lambda is particularly useful for event-driven workloads and short-running functions.

8. AWS KMS — Key Management Service
KMS is used to create and manage cryptographic keys.

It is commonly used with:

S3

EBS

RDS

Secrets and application encryption

Other AWS services

For example:

Application
    ↓
Encrypted data
    ↓
AWS KMS key
KMS isn't simply "a place to store passwords." It primarily manages cryptographic keys and integrates with AWS services for encryption.

9. CloudTrail
CloudTrail answers a different question from CloudWatch.

CloudWatch:

"How is my application/system behaving?"

CloudTrail:

"Who did what in my AWS account?"

For example:

User: John
Action: DeleteSecurityGroup
Time: 10:30 AM
Resource: sg-12345
This is extremely useful for:

Security investigations

Auditing

Compliance

Troubleshooting

Tracking AWS API activity

10. EKS — Elastic Kubernetes Service
EKS is AWS's managed Kubernetes service.

Kubernetes manages containers across multiple machines.

Instead of manually managing a Kubernetes control plane, AWS manages much of that infrastructure for you.

Typical architecture:

             EKS Cluster
                  |
        ---------------------
        |         |         |
       Pod       Pod       Pod
        |         |         |
     Service   Service   Service
EKS is commonly used for larger containerized applications and microservice architectures.

11. ECS — Elastic Container Service
ECS is AWS's own container orchestration service.

You can run Docker containers using ECS.

For example:

ECS Cluster
    |
    +--- Backend container
    |
    +--- Frontend container
    |
    +--- Worker container
ECS is generally simpler and more AWS-specific than Kubernetes/EKS.

A simplified distinction:

ECS = AWS-native container orchestration

EKS = Kubernetes on AWS

12. Fargate
Fargate is serverless compute for containers.

This distinction is important.

With traditional ECS:

ECS
 ↓
EC2
 ↓
Containers
You have to manage the EC2 servers.

With ECS + Fargate:

ECS
 ↓
Fargate
 ↓
Containers
AWS manages the underlying compute infrastructure.

Fargate can also be used with EKS.

So:

ECS/EKS = orchestration

Fargate = compute environment for containers

13. ELK
ELK is not one AWS service. It's a technology stack consisting of:

E — Elasticsearch
L — Logstash
K — Kibana

It is commonly used for centralized logging and log analysis.

Example:

EC2 ───────┐
           │
ECS ───────┼──→ Logstash → Elasticsearch → Kibana
           │
Lambda ────┘
Logstash processes/transforms logs.

Elasticsearch stores and searches the data.

Kibana provides dashboards and visualization.

For example, you could search:

ERROR
service=payment
status=500
last 24 hours
and visualize the results in Kibana.

********************************************************
                     AWS
********************************************************


Compute

EC2       → Virtual machines
Lambda    → Serverless functions
ECS       → Container orchestration
EKS       → Kubernetes
Fargate   → Serverless container compute

Storage

EBS       → Block storage
S3        → Object storage

Networking

VPC       → Virtual network
Subnet    → Network subdivision
Security Group → Instance/resource firewall

Security

IAM       → Identity & permissions
KMS       → Encryption key management
CloudTrail → Audit/API activity

Monitoring & logging

CloudWatch → AWS monitoring/logs/alerts
ELK        → Centralized log search & visualization

AWS ↔ Azure cheat sheet
AWS                         Azure
------------------------------------------------
EC2                    →    Virtual Machines
VPC                    →    Virtual Network (VNet)
EBS                    →    Managed Disks
S3                     →    Blob Storage
IAM                    →    Entra ID + Azure RBAC
CloudWatch             →    Azure Monitor
Lambda                 →    Azure Functions
KMS                    →    Key Vault
CloudTrail             →    Activity Log
EKS                    →    AKS
ECS                    →    Container Apps / ACI
Fargate                →    Serverless container options
ELK                    →    Azure Monitor + Log Analytics / Elas
