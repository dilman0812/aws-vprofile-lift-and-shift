---
title: Build and Deploy Application Artifact
project: AWS vProfile Lift & Shift
---

## Overview

In this step, the application artifact was built locally and deployed to the EC2 instance using Amazon S3.

The process includes:

- Building the application using Maven
- Uploading the artifact to S3
- Downloading the artifact on the EC2 instance
- Deploying it to Tomcat

---

## Source Code

The application source code was cloned from:

```
https://github.com/hkhcoder/vprofile-project
```

---

## Build Artifact

The artifact was built using Maven.

Command used:

```bash
mvn install
```

Build result:

- WAR file generated:

```
target/vprofile-v2.war
```

---

## AWS CLI Configuration

AWS CLI was configured on the local machine using IAM user credentials.

Command used:

```bash
aws configure
```

This enables authentication to upload files to S3.

---

## S3 Bucket

An S3 bucket was created to store the artifact.

```
Bucket Name: vprofile-las-artifactz37
```

---

## Upload Artifact to S3

The artifact was uploaded using AWS CLI.

```bash
aws s3 cp target/vprofile-v2.war s3://vprofile-las-artifactz37/
```

---

## IAM Configuration

### IAM User

- User: vprofile-s3-admin
- Permission: AmazonS3FullAccess
- Used for local machine authentication

### IAM Role

- Role: s3-admin
- Permission: AmazonS3FullAccess
- Attached to EC2 instance (vprofile-app01)

This allows the EC2 instance to access S3 without using credentials.

---

## Deployment on EC2 (Tomcat)

The following steps were performed on the EC2 instance (vprofile-app01).

---

### Install AWS CLI

```bash
snap install aws-cli --classic
```

---

### Download Artifact from S3

```bash
aws s3 cp s3://vprofile-las-artifactz37/vprofile-v2.war /tmp/
```

---

### Stop Tomcat

```bash
systemctl stop tomcat10
systemctl daemon-reload
systemctl stop tomcat10
```

---

### Remove Default Application

```bash
rm -rf /var/lib/tomcat10/webapps/ROOT
```

---

### Deploy New Artifact

```bash
cp /tmp/vprofile-v2.war /var/lib/tomcat10/webapps/ROOT.war
```

---

### Start Tomcat

```bash
systemctl start tomcat10
```

---

### Verification

```bash
ls /var/lib/tomcat10/webapps
```

Output:

```
ROOT  ROOT.war
```

---

## Screenshots

### S3 Bucket

```
screenshots/s3/s3-bucket-created.png
```

---

### Artifact in S3

```
screenshots/s3/s3-artifact-upload.png
```

---

### IAM Role

```
screenshots/s3/iam-role.png
```

---

### IAM Role Attached to EC2

```
screenshots/s3/ec2-iam-role-attached.png
```

---

## Result

The application artifact was successfully:

- Built locally
- Uploaded to S3
- Downloaded on EC2
- Deployed to Tomcat

The application is now running internally on the EC2 instance.

---

## Next Step

```
05-load-balancer.md
```
