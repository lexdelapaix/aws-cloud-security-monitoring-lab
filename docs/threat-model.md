# Threat Model

## Purpose

This threat model identifies the primary security risks the AWS Cloud Security Monitoring Lab is designed to demonstrate.

The objective is to define realistic security scenarios before implementing infrastructure and detection logic.

The environment is intentionally limited in scope and exists within a controlled laboratory environment.

---

## Assets

The primary assets in the environment include:

### AWS Identity and Access

IAM users, roles, policies, and permissions control access to AWS resources.

### Compute Resources

EC2 provides the primary compute environment used by the laboratory.

### Network Configuration

VPC configuration, subnets, routes, and security groups determine how resources communicate and what network access is permitted.

### Security Telemetry

CloudTrail records AWS API activity and provides the primary audit source for cloud security investigations.

### Log Storage

Centralized storage contains security-relevant audit records used for detection and investigation.

---

## Threat Actors

The project considers three primary threat scenarios.

### External Attacker

An external attacker obtains or attempts to use compromised credentials or exposed services to access AWS resources.

### Compromised Identity

A legitimate identity is compromised and used to perform activity that is inconsistent with the identity's intended role.

### Malicious or Compromised Insider

A user with legitimate access intentionally or unintentionally performs actions outside their authorized responsibilities.

---

# Threat Scenarios

## 1. Compromised Credentials

### Scenario

An attacker obtains valid AWS credentials and uses them to authenticate to the environment.

### Potential Impact

A compromised identity could allow an attacker to access resources or perform actions associated with the identity's permissions.

### Detection Goal

Identify authentication activity that is unusual, unexpected, or inconsistent with the intended use of the identity.

### Evidence

Potential evidence includes:

* Authentication events
* Source information
* API activity
* Identity used
* Timestamp
* Subsequent AWS actions

---

## 2. Excessive IAM Permissions

### Scenario

An identity has more permissions than required to perform its intended function.

### Potential Impact

Excessive permissions increase the potential impact of credential compromise or misuse.

### Detection Goal

Identify overly broad permissions and demonstrate the security impact of excessive access.

### Security Control

Apply least-privilege IAM policies and separate administrative permissions from operational access.

---

## 3. IAM Privilege Escalation

### Scenario

A compromised or unauthorized identity attempts to modify IAM permissions or assume a role with greater privileges.

### Potential Impact

Successful privilege escalation could allow unauthorized access to additional AWS resources.

### Detection Goal

Detect IAM policy changes, role changes, or other activity associated with privilege escalation.

### Evidence

Potential evidence includes:

* IAM API calls
* Policy modifications
* Role assumptions
* Identity information
* Source information
* Timestamp
* Related activity before and after the change

---

## 4. Security Group Modification

### Scenario

An identity modifies a security group to allow network access that was not previously permitted.

### Potential Impact

An overly permissive security group could expose an AWS resource to unauthorized network access.

### Detection Goal

Identify changes that increase network exposure.

### Evidence

Potential evidence includes:

* Security group modification events
* Previous configuration
* New configuration
* Identity responsible for the change
* Timestamp
* Source information

---

## 5. Unauthorized Resource Creation

### Scenario

An identity creates an AWS resource outside of the expected operational workflow.

### Potential Impact

Unauthorized resources could be used for persistence, data access, cryptomining, command-and-control infrastructure, or other malicious activity.

### Detection Goal

Identify unexpected resource creation and determine whether the activity was authorized.

### Evidence

Potential evidence includes:

* Resource creation events
* Identity
* Timestamp
* Resource type
* Source information
* Related API activity

---

## 6. Log Tampering

### Scenario

An attacker attempts to disable logging, alter logging configuration, or interfere with the collection of security telemetry.

### Potential Impact

Loss or modification of audit data could reduce the ability to detect and investigate malicious activity.

### Detection Goal

Identify attempts to modify or disable security logging.

### Security Control

Logging configuration should be separated from normal workload permissions wherever practical.

---

# Risk Prioritization

| Threat                         | Likelihood | Potential Impact | Priority |
| ------------------------------ | ---------- | ---------------- | -------- |
| Compromised credentials        | Medium     | High             | High     |
| Excessive IAM permissions      | Medium     | High             | High     |
| IAM privilege escalation       | Medium     | High             | High     |
| Security group modification    | Medium     | High             | High     |
| Unauthorized resource creation | Medium     | Medium           | Medium   |
| Log tampering                  | Low/Medium | High             | High     |

The priorities are based on the potential impact to the laboratory environment and the value of demonstrating detection and response capabilities.

---

# Security Objectives

The project should demonstrate that the environment can:

1. Restrict access using least-privilege IAM controls.
2. Establish network boundaries around resources.
3. Record AWS API activity.
4. Detect meaningful security changes.
5. Preserve sufficient evidence for investigation.
6. Distinguish authorized administrative activity from suspicious activity.
7. Document investigation findings and recommended response.
8. Reproduce infrastructure configuration through infrastructure-as-code.

---

# Investigation Model

Security events will follow this workflow:

```text
Activity
   ↓
Telemetry
   ↓
Detection
   ↓
Initial Triage
   ↓
Evidence Collection
   ↓
Correlation
   ↓
Classification
   ↓
Response Recommendation
   ↓
Lessons Learned
```

The goal is to demonstrate both the **engineering controls** that produce useful security telemetry and the **analytical process** required to interpret that telemetry.
