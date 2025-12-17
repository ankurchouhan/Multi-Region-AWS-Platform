
# 🌍 Production-Hardened Multi-Region AWS Platform (Terraform)

**Enterprise-grade, ready-to-run AWS platform blueprint**  
Built for **high availability, global scale, security, disaster recovery, and cost optimization**

---

## ✨ Overview

This repository provides a **production-hardened Terraform reference architecture** for running mission-critical workloads on AWS across multiple regions.

It combines two proven patterns:

- 🌐 **Global High-Availability Load Balancing** (CloudFront + Route53 + ALB)
- 🏗️ **Multi-Region Application Platform** (ECS, DR, CI/CD, Security)

Designed for teams that **expect failure** and engineer systems that continue to operate through **regional outages, traffic spikes, and deployment errors**.

---

## 🏗️ Architecture Highlights

- CloudFront as the global **Tier-0 entry point**
- AWS WAF + Shield Advanced for edge security and DDoS protection
- Multi-Region ECS (Fargate)
- Cost-optimized **Active-Passive DR**
- Route53 latency & health-based routing
- Cross-region encrypted backups
- WebSocket real-time support
- GitHub Actions CI/CD pipelines

---

## 📐 Full AWS Architecture & CI/CD Diagram

```mermaid
flowchart TB
    User[Users / Clients]
    CF[CloudFront<br/>Global LB]
    WAF[AWS WAF<br/>Shield Advanced]
    R53[Route53<br/>Latency + Health]

    subgraph us-east-1
        ALB1[Public ALB]
        ECS1[ECS Fargate]
        IALB1[Internal ALB]
        DB1[Aurora Writer]
        SM1[Secrets Manager]
    end

    subgraph eu-west-1
        ALB2[Public ALB]
        ECS2[ECS Fargate]
        IALB2[Internal ALB]
        DB2[Aurora Read Replica]
        SM2[Secrets Manager]
    end

    WS[API Gateway<br/>WebSocket]
    LAMBDA[Lambda<br/>Conn Mgmt]

    DEV[Developer]
    GH[GitHub]
    GA[GitHub Actions]
    ECR[ECR]
    CD[CodeDeploy]

    BK[AWS Backup]
    DR[DR Region]

    User --> CF --> WAF --> R53
    R53 --> ALB1 --> ECS1 --> IALB1 --> DB1
    R53 --> ALB2 --> ECS2 --> IALB2 --> DB2

    User --> WS --> LAMBDA --> ECS1
    User --> WS --> LAMBDA --> ECS2

    DEV --> GH --> GA --> ECR
    GA --> CD --> ECS1
    GA --> CD --> ECS2

    SM1 --> ECS1
    SM2 --> ECS2

    DB1 --> BK --> DR
```

---

## 🚀 CI/CD Flow

1. Developer pushes code to GitHub
2. GitHub Actions builds container
3. Image pushed to ECR
4. CodeDeploy performs ECS Blue/Green
5. Traffic shifted via ALB
6. Automatic rollback on alarms

---

## 🔒 Security

- CloudFront + AWS WAF
- Shield Advanced
- Private subnets only
- No public compute
- Secrets Manager per region

---

## 🔁 Disaster Recovery

Active-Passive using Route53 health checks.  
DR region runs **zero steady-state compute**.

---

## 💥 Chaos Engineering

- AWS Fault Injection Simulator
- ECS task termination
- AZ impairment
- Regional failover tests

---

## 📜 License

MIT License
