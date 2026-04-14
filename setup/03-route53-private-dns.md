---
title: Route 53 Private DNS Setup
project: AWS vProfile Lift & Shift
---

## Overview

In a cloud environment, using IP addresses directly in application configuration is not reliable.

If an instance is replaced, its IP address changes, requiring manual updates in the application.

To solve this, a DNS-based approach is used for service communication.

---

## Problem

Using IP addresses in configuration leads to:

- Manual updates when instances are replaced
- Tight coupling between application and infrastructure
- Reduced flexibility

---

## Solution

A private DNS system was set up using Amazon Route 53.

Services communicate using domain names instead of IP addresses, and Route 53 resolves those names to the corresponding private IPs.

---

## Hosted Zone

A private hosted zone was created with the following configuration:

```
Domain Name: vprofile.in
Type: Private Hosted Zone
Region: us-east-1
```

This hosted zone is associated with the VPC and enables internal DNS resolution between instances.

---

## DNS Records

The following records were created:

```
db01.vprofile.in   → Database instance private IP
mc01.vprofile.in   → Memcached instance private IP
rmq01.vprofile.in  → RabbitMQ instance private IP
```

These records allow the application to refer to backend services using stable names.

---

## Usage

The application will use:

```
db01.vprofile.in
mc01.vprofile.in
rmq01.vprofile.in
```

Instead of:

```
Hardcoded IP addresses
```

---

## Screenshots

### Hosted Zone

```
screenshots/route53/route53-hosted-zone.png
```

---

### DNS Records

```
screenshots/route53/route53-records.png
```

---

## Next Step

```
04-build-deploy-artifact.md
```
