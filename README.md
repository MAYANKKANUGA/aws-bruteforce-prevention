# AWS Brute Force Prevention — Project Documentation

**Author:** Mayank Kanuga  
**GitHub:** [github.com/MAYANKKANUGA/aws-bruteforce-prevention](https://github.com/MAYANKKANUGA/aws-bruteforce-prevention)  
**Stack:** AWS EC2 · Systems Manager · GuardDuty · EventBridge · SNS · IAM

---

## Objective

Eliminate the SSH/RDP brute-force attack surface on AWS EC2 instances by:

1. Removing all open inbound ports (zero open-port architecture).
2. Replacing SSH/RDP with AWS Systems Manager (SSM) Session Manager.
3. Detecting brute-force attempts automatically via GuardDuty.
4. Alerting in real-time via Amazon EventBridge and SNS.
5. Validating the full detection chain with a live Hydra attack simulation.

---

## Architecture Overview

```text
Attacker (Hydra) 
       │
       ▼
EC2 (No Open Ports)
Security Group: Inbound = DENY ALL
       │
SSM Session Manager
(Port-free shell access via IAM role)
       │
GuardDuty Detector (Analyzes VPC Flow Logs)
├── Finding: UnauthorizedAccess:EC2/SSHBruteForce
└── Finding: UnauthorizedAccess:EC2/RDPBruteForce
       │
Amazon EventBridge
(Filters GuardDuty findings and triggers target)
       │
SNS Topic ──► Real-Time Email Alert# aws-bruteforce-prevention
A zero-trust AWS EC2 architecture utilizing SSM Session Manager, GuardDuty, EventBridge, and SNS to automatically detect and alert on SSH brute-force attacks.
