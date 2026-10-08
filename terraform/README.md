# Terraform Infrastructure

This directory contains the infrastructure-as-code for the AWS Cloud Security Monitoring Lab.

The goal is to create a small, reproducible AWS environment that demonstrates cloud security controls, centralized logging, and security monitoring.

## Infrastructure Components

### IAM

IAM resources will establish controlled identities and permissions for the laboratory.

Planned controls include:

* Least-privilege policies
* Dedicated roles where appropriate
* Separation of administrative and workload permissions
* Controlled permission changes
* Auditable identity activity

No credentials or secrets will be stored in Terraform configuration.

### VPC

The lab will use a dedicated VPC to establish a controlled network boundary.

Planned components include:

* VPC
* Subnet(s)
* Internet gateway where required
* Route configuration
* Security groups

Network access will be limited to the minimum required for the laboratory.

### EC2

A small EC2 instance will provide the primary compute environment.

The instance will be configured to support:

* Secure administrative access
* Security testing
* Controlled activity generation
* Monitoring and investigation

The smallest practical instance size will be used to reduce unnecessary cost.

### CloudTrail

CloudTrail will capture AWS API activity for the environment.

The logging configuration will support investigation of:

* IAM activity
* Authentication events
* Resource creation
* Resource modification
* Security group changes
* Administrative actions

### S3

An S3 bucket will provide centralized storage for CloudTrail logs.

The bucket will be configured with security controls intended to protect audit data from unauthorized modification or deletion.

## Infrastructure Flow

```text
Terraform
   │
   ├── IAM
   │
   ├── VPC
   │     └── Security Groups
   │
   ├── EC2
   │
   ├── CloudTrail
   │
   └── S3
         │
         ▼
     Audit Logs
         │
         ▼
     Detection
         │
         ▼
   Investigation
```

## Security Requirements

The infrastructure should meet the following requirements:

### Identity

* Avoid unnecessary long-lived credentials.
* Apply least privilege.
* Separate administrative and workload permissions.
* Avoid wildcard permissions unless technically justified.

### Network

* Avoid unrestricted inbound access.
* Restrict administrative access to known sources where practical.
* Do not expose unnecessary services.
* Document intentional network exposure.

### Compute

* Use the smallest practical instance.
* Keep unnecessary services disabled.
* Apply appropriate operating-system security controls.
* Avoid storing sensitive information on the instance.

### Logging

* Enable CloudTrail.
* Store logs centrally.
* Protect log storage from unauthorized changes.
* Ensure security-relevant activity can be correlated with an identity and timestamp.

## Cost Management

The environment is designed as a laboratory rather than a continuously running production environment.

Resources should be stopped or destroyed when they are not actively being used.

Terraform should be used to make deployment and cleanup reproducible.

## Sensitive Information

The repository must never contain:

* AWS access keys
* Secret keys
* Passwords
* Private keys
* Session tokens
* API secrets
* Terraform state containing sensitive information

Sensitive local configuration files must be excluded through `.gitignore`.

## Deployment Strategy

Infrastructure will be developed incrementally.

The initial deployment will establish:

1. AWS provider configuration
2. IAM controls
3. VPC networking
4. Security group controls
5. EC2 compute
6. CloudTrail logging
7. Protected S3 log storage

Security validation will occur after each major layer rather than waiting until the entire environment is deployed.


