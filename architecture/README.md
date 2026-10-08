# Architecture

## Overview

The AWS Cloud Security Monitoring Lab is designed to demonstrate the implementation of security controls, centralized logging, monitoring, detection, and investigation within an AWS environment.

The project focuses on the security engineering lifecycle:

**Design → Provision → Secure → Monitor → Detect → Investigate → Improve**

The environment will be built using infrastructure-as-code where practical so that infrastructure configuration is reproducible and auditable.

## Architecture

```text
                         AWS ACCOUNT
                              │
                 ┌────────────┴────────────┐
                 │                         │
                 ▼                         ▼
              IAM                     CloudTrail
        Identity & Access             Audit Logging
                 │                         │
                 │                         ▼
                 │                    Log Storage
                 │                         │
                 ▼                         │
                VPC                       │
                 │                         │
        ┌────────┴────────┐                │
        │                 │                │
        ▼                 ▼                │
   Network Controls   Compute              │
        │                 │                │
        │                 ▼                │
        │                EC2               │
        │                 │                │
        └─────────────────┴────────────────┘
                          │
                          ▼
                  Security Monitoring
                          │
                          ▼
                     Detection
                          │
                          ▼
                    Investigation
```

## Core Components

### IAM

IAM provides identity and access controls for the environment.

The project will demonstrate:

* Least-privilege access
* Role-based access
* Separation of administrative and operational permissions
* Analysis of authentication and authorization events
* Auditable permission changes

Credentials and long-lived access keys will not be stored in the repository.

### VPC

The VPC provides the network boundary for the environment.

The design will demonstrate:

* Network segmentation
* Subnets
* Route controls
* Security groups
* Restricted inbound and outbound access

Network access will be limited to what is required for the lab.

### EC2

EC2 will provide a controlled compute environment for generating and observing security-relevant activity.

The instance will be configured with security and operational controls appropriate for a laboratory environment.

### CloudTrail

CloudTrail will provide an audit record of AWS API activity.

The project will use CloudTrail telemetry to investigate activities such as:

* Authentication events
* IAM changes
* Security group modifications
* Resource creation
* Resource modification
* Administrative activity

### Log Storage

Cloud activity logs will be stored separately from the compute environment to support centralized auditing and investigation.

Storage controls will be configured to reduce the risk of unauthorized modification or deletion of audit data.

### Detection

Detection logic will identify activity that may indicate:

* Unauthorized access
* Privilege escalation
* Suspicious IAM changes
* Unexpected administrative activity
* Network security changes
* Unusual resource activity

### Investigation

Detected activity will be investigated by correlating relevant telemetry and determining whether the activity is:

* Expected
* Authorized
* Suspicious
* Malicious

Each investigation will document the evidence, reasoning, classification, and recommended response.

## Security Design Principles

The environment will follow these principles:

### Least Privilege

Access should provide only the permissions required to perform an intended function.

### Defense in Depth

Security controls will exist across identity, network, compute, logging, and monitoring layers rather than relying on a single control.

### Centralized Logging

Security-relevant activity should be captured and retained separately from the systems generating the activity.

### Auditability

Infrastructure changes and administrative actions should produce sufficient evidence for later review.

### Reproducibility

Infrastructure configuration should be defined through code where practical rather than relying exclusively on manual configuration.

### Secure-by-Default Configuration

Unnecessary services, permissions, network exposure, and credentials should be avoided.

## Threat Considerations

The environment will consider threats including:

* Compromised credentials
* Excessive IAM permissions
* Privilege escalation
* Unauthorized administrative activity
* Network exposure
* Security group misconfiguration
* Resource tampering
* Log tampering or deletion

## Project Scope

This is a controlled cybersecurity laboratory environment.

The goal is not to reproduce a complete enterprise AWS security platform. The goal is to demonstrate the engineering decisions required to design, secure, monitor, and investigate a cloud environment.

