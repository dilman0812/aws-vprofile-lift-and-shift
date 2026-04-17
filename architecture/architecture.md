---
title: AWS Architecture
project: AWS vProfile Lift & Shift
---

## Overview

This project migrates the **vProfile multi-tier application stack** from a locally provisioned infrastructure (Vagrant + VirtualBox) to **AWS Cloud** using a **Lift & Shift strategy**.

The goal is to replicate the existing application architecture on AWS while leveraging cloud-native infrastructure for scalability, security, and operational simplicity.

The migration focuses on **infrastructure transformation rather than application modification**.

---

## Original Local Architecture

In the previous project (`vprofile-local-devops-stack`), the application stack was provisioned locally using Vagrant.

### Application Components

| Layer | Service | Purpose |
|---|---|---|
Web | Nginx | Reverse proxy and entry point |
Application | Apache Tomcat | Java application server |
Messaging | RabbitMQ | Message broker |
Cache | Memcached | Data caching |
Database | MySQL | Persistent storage |

### Local Request Flow

```
Browser
  ↓
Nginx
  ↓
Tomcat
  ├── MySQL
  ├── Memcached
  └── RabbitMQ
```

---

## AWS Migration Strategy

The migration follows a **Lift & Shift approach**, meaning:

- The application architecture remains largely unchanged
- Infrastructure moves from **local VMs → AWS cloud infrastructure**
- Services are hosted on **EC2 instances**

This approach enables quick migration while maintaining compatibility with the existing system.

---

## Target AWS Architecture

The AWS deployment replaces the local infrastructure components with cloud-managed resources.

### Key AWS Components

| Component | AWS Service |
|---|---|
Compute | EC2 Instances |
Load Balancing | Application Load Balancer (ALB) |
Scaling | Auto Scaling Group |
Storage | Amazon S3 |
DNS | Route 53 |
Certificates | AWS Certificate Manager |
Security | Security Groups |
Authentication | IAM (User + Role) |

---

## Request Flow

```
User
 ↓
Domain (GoDaddy DNS - CNAME)
 ↓
Application Load Balancer (HTTPS via ACM)
 ↓
Auto Scaling Group (Tomcat EC2 Instances)
 ↓
Route 53 Private DNS
   ├── db01.vprofile.in → MySQL
   ├── mc01.vprofile.in → Memcached
   └── rmq01.vprofile.in → RabbitMQ
```

---

## Artifact Flow

The application deployment follows a build and artifact distribution model:

```
Local Machine
   ↓
Build (Maven)
   ↓
WAR Artifact
   ↓
Amazon S3 (Artifact Storage)
   ↓
EC2 (Tomcat Instances)
   ↓
Application Deployment
```

This separates build and runtime environments and enables consistent deployments.

---

## Security Architecture

The infrastructure follows a **three-tier security model**:

```
Internet
   ↓
vprofile-ELB-SG
   ↓
vprofile-app-sg
   ↓
vprofile-backend-sg
```

### Security Groups

| Security Group | Purpose |
|---|---|
vprofile-ELB-SG | Public entry point (ALB) |
vprofile-app-sg | Application servers (Tomcat) |
vprofile-backend-sg | Backend services (DB, Cache, Messaging) |

This layered model ensures:

- Public access only to the load balancer
- Restricted communication between application tiers
- Backend services remain private

---

## Infrastructure Deployment Phases

The infrastructure was deployed incrementally in the following stages:

```
1. Security Groups & Keypairs
2. EC2 Instances
3. Route 53 Private DNS
4. Build & Deploy Artifacts
5. Load Balancer Configuration
6. Auto Scaling Group
7. Application Validation
```

Each stage is documented separately in the `/setup` directory.

---

## Architecture Diagram

The final AWS architecture diagram is available at:

```
architecture/aws-architecture.png
```

---

## Outcome

After completing this migration, the vProfile application runs on AWS with:

- Scalable infrastructure
- Load-balanced traffic distribution
- Automated instance scaling
- Secure network segmentation
- Cloud-based artifact storage (S3)
- DNS-based service discovery (Route 53)

This demonstrates a practical **Lift & Shift migration of a multi-tier application workload to AWS Cloud**.
