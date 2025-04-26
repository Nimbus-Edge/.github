# Nimbus-Edge: Cloud Native Web Application Platform

Nimbus-Edge is a secure, resilient, and scalable cloud-native platform built for modern application needs. It combines a Spring Boot microservice backend, an AWS Lambda-based email verification service, and a Terraform-driven AWS infrastructure, fully automated through CI/CD pipelines for seamless deployments.

---

## 🧱 System Architecture

```
                        ┌────────────────────────────────────────────┐
                        │        Route 53 / SSL Terminated ALB        │
                        └────────────────────────────────────────────┘
                                       │
                                       ▼
                            ┌──────────────────────┐
                            │ Spring Boot Backend  │
                            │  - Auth, DB, S3 Ops  │
                            └──────────────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │ SNS Topic: Email Notifications │
                        └──────────────────────────────┘
                                       │
                                       ▼
                      ┌─────────────────────────────────────┐
                      │ Lambda: Email Verification Service  │
                      │  - Sends Styled HTML Verification   │
                      └─────────────────────────────────────┘
```

---

## 📦 Repositories

| Repository | Description |
|:-----------|:------------|
| [`webapp`](./webapp/README.md) | Spring Boot REST API backend for user management, S3 uploads, and authentication. |
| [`serverless`](./serverless/README.md) | AWS Lambda serverless function triggered via SNS to send verification emails using SendGrid. |
| [`tf-aws-infra`](./tf-aws-infra/README.md) | Full Infrastructure as Code setup with Terraform for AWS VPC, EC2, RDS, S3, ALB, IAM, SNS, Lambda, and CloudWatch. |

---

## 🌐 Components Overview

### Spring Boot Backend (`webapp`)
- User registration, login, profile updates
- Profile image management with AWS S3
- Health checks, structured logging, metrics
- Basic authentication with email verification flow
- PostgreSQL database (or MySQL, configurable)
- Deployment artifacts built and baked into custom AMIs
- Application logs stored in CloudWatch

### Lambda Email Verification Service (`serverless`)
- Node.js 18 Lambda function
- Securely fetches SendGrid API key from AWS Secrets Manager
- Listens to SNS notifications from the backend
- Sends HTML-styled email verification links
- Detailed error handling and CloudWatch logging

### Terraform Infrastructure (`tf-aws-infra`)
- Modularized Terraform setup following AWS best practices
- Multi-AZ VPC with public/private subnets
- ALB with SSL termination
- Auto Scaling Groups with Launch Templates and rolling updates
- PostgreSQL RDS instance in private subnets
- S3 buckets for secure storage with lifecycle policies
- SNS + Lambda integration
- Secrets Manager and KMS for encryption
- CloudWatch alarms, logs, and metrics
- GitHub Actions CI/CD pipelines for infrastructure and application deployments

---

## 🔒 Security Best Practices

- SSL/TLS everywhere (Route 53 + ACM)
- Private subnets for sensitive resources (DB, EC2)
- IAM least-privilege access for Lambda, EC2, RDS
- S3 server-side encryption
- Secrets stored and rotated in AWS Secrets Manager
- KMS keys with automatic rotation enabled
- Structured error handling and logging

---

## 🚀 Deployment Workflow

1. Developer pushes code to GitHub `main` branch.
2. GitHub Actions pipeline triggers:
   - Spring Boot application: Build → Test → Artifact creation → AMI bake via Packer → Rolling ASG update.
   - Terraform modules: Format, validate, plan, apply.
3. Infrastructure dynamically updates via version-controlled Terraform plans.
4. EC2 Auto Scaling Group refreshes instances with zero downtime.
5. Email verification Lambda is auto-triggered by user registration events via SNS.

---

## 📊 Monitoring and Observability

- Spring Boot Actuator endpoints
- Application logs centralized to CloudWatch
- CloudWatch Alarms for critical metrics (CPU, memory, etc.)
- Auto Scaling policies based on real-time load
- Lambda metrics and invocation tracking
- Database performance insights via RDS Enhanced Monitoring

---

## 📋 Prerequisites

- AWS Account (IAM access to EC2, RDS, S3, SNS, Secrets Manager, ACM, CloudWatch, Lambda)
- GitHub account (for code and CI/CD)
- Terraform CLI (v1.9+)
- Java 17+
- Node.js 18+
- SendGrid account for transactional emails
- Route 53 domain and SSL certificates
- PostgreSQL or MySQL client for DB setup

---

## 📈 Future Enhancements (Roadmap)

- JWT-based authentication (access + refresh tokens)
- WebSocket-based notifications (e.g., account verified notifications)
- Blue/Green or Canary deployments for zero-downtime rollouts
- API Gateway integration for Lambda microservices
- Redis caching layer for session and token management
- Fine-grained cost monitoring and alerting (AWS Budgets, CloudWatch)

---

## 📜 License

This project is licensed under the MIT License.
