---
title: Security Groups and Keypairs
project: AWS vProfile Lift & Shift
---

## Overview

Before launching any compute infrastructure, the network access rules and authentication mechanism must be defined.

In AWS, this is achieved using:

- **Security Groups** → Stateful virtual firewalls controlling traffic
- **Key Pairs** → Secure SSH authentication for EC2 instances

For this project, security groups were designed to mirror the **multi-tier architecture of the vProfile application stack**.

---

## AWS Region

The infrastructure for this project is deployed in:

```
us-east-1 (N. Virginia)
```

This region was selected because it is one of the most commonly used AWS regions and provides broad service availability.

---

## Key Pair Creation

A key pair was created to securely access the EC2 instances.

```
Key Pair Name: vprofile-prod-key
Purpose: SSH authentication for EC2 instances
```

AWS generates a **.pem private key file** that must be stored securely and used when connecting to instances.

Example connection command:

```bash
ssh -i vprofile-prod-key.pem ec2-user@<EC2_PUBLIC_IP>
```

### Screenshot

```
screenshots/keypairs/vprofile-keypair.png
```

---

## Security Group Design

Three security groups were created to isolate traffic between different tiers of the application.

| Security Group | Purpose |
|---|---|
| vprofile-ELB-SG | Load balancer entry point |
| vprofile-app-sg | Application servers (Tomcat) |
| vprofile-backend-sg | Database and backend services |

This layered approach ensures **controlled and secure communication between services**.

---

## 1. ELB Security Group

```
Security Group: vprofile-ELB-SG
Purpose: Public entry point for web traffic
```

### Inbound Rules

| Protocol | Port | Source | Purpose |
|---|---|---|---|
HTTP | 80 | 0.0.0.0/0 | Allow public HTTP traffic |
HTTP | 80 | ::/0 | Allow IPv6 HTTP traffic |
HTTPS | 443 | 0.0.0.0/0 | Allow secure HTTPS traffic |
HTTPS | 443 | ::/0 | Allow IPv6 HTTPS traffic |

HTTPS is allowed because later the load balancer will use **AWS Certificate Manager (ACM)** to provide secure SSL termination.

### Screenshot

```
screenshots/security-groups/elb-sg-inbound.png
```

---

## 2. Application Security Group

```
Security Group: vprofile-app-sg
Purpose: Apache Tomcat application servers
```

### Inbound Rules

| Protocol | Port | Source | Purpose |
|---|---|---|---|
TCP | 8080 | vprofile-ELB-SG | Allow traffic from Load Balancer |
TCP | 22 | My IP | Secure SSH administration |

Only the load balancer is allowed to access the application servers, enforcing **tier isolation**.

### Screenshot

```
screenshots/security-groups/app-sg-rules.png
```

---

## 3. Backend Security Group

```
Security Group: vprofile-backend-sg
Purpose: Database and backend services
```

These instances host:

- MySQL
- Memcached
- RabbitMQ

### Inbound Rules

| Protocol | Port | Source | Purpose |
|---|---|---|---|
TCP | 3306 | vprofile-app-sg | MySQL database access |
TCP | 11211 | vprofile-app-sg | Memcached access |
TCP | 5672 | vprofile-app-sg | RabbitMQ messaging |
TCP | 22 | My IP | SSH administration |
All Traffic | All | vprofile-backend-sg | Backend services communication |

Allowing **all traffic within the security group** ensures backend services can communicate with each other without restrictions.

### Screenshot

```
screenshots/security-groups/backend-sg-rules.png
```

---

## Security Architecture Summary

The security model follows a **layered architecture**:

```
Internet
   ↓
ELB Security Group
   ↓
Application Security Group (Tomcat)
   ↓
Backend Security Group (MySQL, Memcached, RabbitMQ)
```

This design ensures:

- Public access only to the load balancer
- Restricted access to application servers
- Internal-only communication for backend services

---

## Next Step

With security controls in place, the next phase is to launch the infrastructure.

Next step:

```
02-ec2-instances.md
```

In this step we will:

- Launch EC2 instances
- Configure user data scripts
- Prepare backend services
- Prepare application servers
