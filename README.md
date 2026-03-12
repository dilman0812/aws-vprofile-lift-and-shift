# AWS vProfile Lift & Shift Migration

## Overview

This project demonstrates the **migration of a multi-tier web application (vProfile)** from a locally provisioned infrastructure to **AWS Cloud** using a **Lift & Shift strategy**.

The goal is to replicate a production-style environment on AWS while improving:

- Infrastructure flexibility
- Scalability
- Cost efficiency
- Operational simplicity

The project focuses on **cloud infrastructure design and deployment**, not application development.

---

## Background

In a previous project, the vProfile application stack was deployed locally using:

- Vagrant
- VirtualBox
- Shell provisioning scripts

Repository:

```
vprofile-local-devops-stack
```

That setup simulated a production environment locally.

However, running such infrastructure in a **traditional data center** introduces several challenges:

- Complex infrastructure management
- High upfront hardware cost
- Limited scalability
- Manual operations
- Difficult automation

Cloud platforms like AWS solve these challenges by providing **Infrastructure as a Service (IaaS)**.

---

## Lift & Shift Strategy

Lift & Shift is a migration approach where an existing application is moved to the cloud **without major architectural changes**.

Instead of redesigning the application:

```
Local VMs → AWS EC2 Instances
```

This allows organizations to migrate workloads quickly while maintaining compatibility with existing systems.

---

## Application Architecture

The vProfile application consists of several services working together.

| Layer | Service | Purpose |
|---|---|---|
Web | Load Balancer | Entry point |
Application | Apache Tomcat | Java application server |
Messaging | RabbitMQ | Message broker |
Cache | Memcached | Performance optimization |
Database | MySQL | Persistent storage |

---

## AWS Services Used

This project uses several AWS services to host and operate the application infrastructure.

| AWS Service | Purpose |
|---|---|
EC2 | Virtual machines running application services |
Application Load Balancer | Traffic distribution |
Auto Scaling | Automatic scaling of application servers |
S3 | Artifact storage |
Route 53 | Private DNS for service discovery |
AWS Certificate Manager | SSL/TLS certificates |
Security Groups | Network security |
EBS | Instance storage |

---

## Deployment Workflow

The infrastructure is deployed in stages.

```
1. Security Groups and Keypairs
2. EC2 Instances
3. Route 53 Private DNS
4. Build and Deploy Application Artifact
5. Load Balancer Setup
6. Auto Scaling Group Configuration
7. Application Validation
```

Each stage is documented inside the `/setup` directory.

---

## Repository Structure

```
aws-vprofile-lift-and-shift
│
├── README.md
│
├── architecture
│   └── architecture.md
│
├── docs
│
├── setup
│   ├── 01-security-groups-keypairs.md
│
├── scripts
│
└── screenshots
```

---

## Current Progress

Completed:

```
✔ Security Groups
✔ Key Pair Setup
```

Next Step:

```
EC2 Instance Deployment
```

---

## Learning Outcomes

This project demonstrates:

- Cloud infrastructure design
- AWS networking and security
- Multi-tier application deployment
- Load balancing and scaling
- Infrastructure migration strategies
- Practical DevOps cloud workflows

---

## Future Enhancements

- Infrastructure as Code (Terraform)
- CI/CD pipeline integration
- Containerization (Docker)
- Kubernetes orchestration
- Monitoring and observability

---

## Author

Built as part of a **DevOps learning journey**, focusing on practical cloud infrastructure implementation and real-world deployment patterns.
