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

## AWS Migration Strategy

The migration follows a **Lift & Shift approach**, meaning:

- The application architecture remains largely unchanged
- Infrastructure moves from **local VMs → AWS cloud infrastructure**
- Services are hosted on **EC2 instances**

This approach enables quick migration while maintaining compatibility with the existing system.

---

## Target AWS Architecture (Planned)

The AWS deployment replaces the local infrastructure components with cloud-managed resources.

### Key AWS Components

| Component | AWS Service |
|---|---|
Compute | EC2 Instances |
Load Balancing | Application Load Balancer (ELB) |
Scaling | Auto Scaling Group |
Storage | S3 / EFS |
DNS | Route 53 |
Certificates | AWS Certificate Manager |
Security | Security Groups |
Authentication | EC2 Key Pairs |

---

## Planned Request Flow

```
User
 ↓
Domain (GoDaddy DNS)
 ↓
Application Load Balancer (HTTPS)
 ↓
Tomcat EC2 Instances (Auto Scaling Group)
 ↓
Backend Services
   ├── MySQL
   ├── Memcached
   └── RabbitMQ
```

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
vprofile-ELB-SG | Public entry point |
vprofile-app-sg | Application servers |
vprofile-backend-sg | Backend services |

This layered model ensures:

- Public access only to the load balancer
- Restricted communication between application tiers
- Backend services remain private

---

## Infrastructure Deployment Phases

The infrastructure is deployed incrementally in the following stages:

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

The final AWS architecture diagram will be added after completing all infrastructure components.

Future diagram location:

```
architecture/aws-architecture.png
```

---

## Outcome

After completing this migration, the vProfile application will run on AWS with:

- Scalable infrastructure
- Load-balanced traffic distribution
- Automated instance scaling
- Secure network segmentation
- Cloud-based artifact storage

This demonstrates a practical **Lift & Shift migration of a multi-tier application workload to AWS Cloud**.
