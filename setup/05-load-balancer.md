---
title: Application Load Balancer Setup
project: AWS vProfile Lift & Shift
---

## Overview

In this step, an Application Load Balancer (ALB) was configured to expose the application to users.

The load balancer distributes traffic to the Tomcat application instance and enables secure access using HTTPS.

---

## Target Group

A target group was created to route traffic to the application instance.

```
Name: vprofile-las-tg
Protocol: HTTP
Port: 8080
```

The target group forwards requests from the load balancer to the Tomcat instance.

---

## Load Balancer

An Application Load Balancer was created with the following configuration:

```
Name: vprofile-las-elf
Security Group: vprofile-ELB-SG
```

---

## Listeners

### Initial Configuration

- HTTP listener on port 80
- Forwarded traffic to the target group

### Final Configuration

- HTTPS listener on port 443
- SSL certificate attached using AWS Certificate Manager
- Traffic forwarded to the target group

---

## SSL Certificate

An SSL certificate was requested and configured using AWS Certificate Manager.

This enables secure HTTPS communication between users and the load balancer.

---

## Domain Mapping

A custom domain was configured using GoDaddy.

A CNAME record was created:

```
Name: vprofileapp
Value: <ALB DNS name>
```

This maps the domain to the load balancer.

---

## Access URLs

The application was successfully accessed using:

### Load Balancer DNS

```
http://vprofile-las-elf-1015430787.us-east-1.elb.amazonaws.com/
```

---

### Custom Domain (HTTPS)

```
https://vprofileapp.dev-dilman.online/
```

---

## Screenshots

### Target Group and Health

```
screenshots/alb/alb-target-group.png
```

---

### Load Balancer Overview

```
screenshots/alb/alb-overview.png
```

---

### Domain Access (HTTPS)

```
screenshots/alb/domain-https-working.png
```

---

## Result

The application is now:

- Accessible via a public URL
- Load balanced through ALB
- Secured using HTTPS
- Mapped to a custom domain

---

## Next Step

```
06-autoscaling-group.md
```
