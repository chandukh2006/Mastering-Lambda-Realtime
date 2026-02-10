# 🔐 IAM Access Key Security Automation (Lower Environments Only)

> **Automated detection, alerting, and deactivation of stale IAM access keys using AWS Lambda, EventBridge, SNS, and IAM**

---

## 📌 Overview

This project implements an **end-to-end AWS security automation** that continuously audits IAM access keys and **automatically deactivates keys that exceed a defined age threshold**. It also sends **real-time notifications** to security and DevOps teams whenever an action is taken.

The solution is intentionally designed for **lower environments only** (sandbox, dev, pre-prod) where developers often generate temporary IAM access keys and forget to rotate or delete them — a common **security and compliance risk**.

---

## 🚨 Why This Matters (Problem Statement)

In real-world cloud environments:

- Developers create IAM access keys for testing
- Keys remain active far beyond their intended lifespan
- Long-lived credentials increase the risk of:
  - Credential leakage
  - Unauthorized access
  - Audit and compliance failures

Manual reviews do not scale.

👉 **This automation enforces security hygiene without human intervention.**

---

## ❗ Important Disclaimer

⚠️ **DO NOT USE IN PRODUCTION ENVIRONMENTS**

Why?

- Production applications or microservices may rely on IAM access keys
- Automatic deactivation could cause **service outages**

✔️ Recommended for:

- Sandbox accounts
- Lower environments
- Security training / demos
- Compliance readiness testing

---

## 🏗️ Architecture

```
EventBridge (Scheduled Rule)
        ↓
AWS Lambda (IAM Key Auditor)
        ↓
IAM APIs (List / Update Keys)
        ↓
SNS Topic → Email Alerts
```

---

## 🧠 Design Principles

- **Automation-first security**
- **Least privilege (recommended)**
- **No hardcoded credentials**
- **Serverless & cost-efficient**
- **Audit-friendly & observable**

---

## 🧩 Components Used

| Service         | Purpose                        |
| --------------- | ------------------------------ |
| IAM             | User and access key management |
| Lambda          | Core automation engine         |
| EventBridge     | Scheduled execution            |
| SNS             | Alerting & notifications       |
| CloudWatch Logs | Execution visibility           |

---

## ⚙️ How It Works (Execution Flow)

1. EventBridge triggers the Lambda function on a schedule
2. Lambda lists all IAM users
3. For each user, it retrieves access keys
4. Active keys are evaluated based on age
5. Keys exceeding the threshold are:
   - Deactivated automatically
   - Logged for audit purposes

6. SNS sends a detailed alert email

---

## 🧪 Testing Configuration

For demonstration and testing:

```bash
MAX_AGE_DAYS = 0
```

This forces immediate deactivation of **any active key**, making it easy to validate behavior.

➡️ **Production-like behavior:** Set to `90` or as per security policy

---

## 🧑‍💻 Lambda Function (Core Logic)

Key responsibilities:

- Enumerate IAM users
- Identify active access keys
- Calculate key age
- Deactivate keys exceeding threshold
- Notify via SNS

Security note:

- No credentials stored in code
- Uses IAM execution role

---

## 🔐 IAM Execution Role Permissions

### Required Actions

```json
{
  "iam:ListUsers",
  "iam:ListAccessKeys",
  "iam:UpdateAccessKey",
  "sns:Publish",
  "logs:CreateLogGroup",
  "logs:CreateLogStream",
  "logs:PutLogEvents"
}
```

⚠️ For demo purposes, permissions are broad.

✅ **Best Practice:** Restrict to least privilege in real environments.

---

## ⏱️ EventBridge Scheduler

- Schedule: Every 1 minute (testing)
- Recommended:
  - Daily or weekly for real usage

The rule invokes Lambda automatically without manual intervention.

---

## 📧 Notification Sample

```
Subject: IAM Access Key Audit Alert

Disabled IAM access keys:

lambda-user (AKIAxxxx) - 0 days
```

---

## 📊 Observability & Auditing

- CloudWatch Logs capture:
  - Execution time
  - Keys evaluated
  - Keys deactivated

- SNS ensures real-time awareness

This makes the solution **audit-ready**.

---

## 🏆 Key Achievements

✅ Automated IAM key lifecycle enforcement
✅ Reduced manual security overhead
✅ Demonstrated real-world DevOps security automation
✅ Serverless, scalable, and cost-efficient

---

## 🎯 Skills Demonstrated (Resume / Portfolio)

- AWS IAM security
- Serverless automation
- Event-driven architecture
- Cloud security best practices
- Operational observability
- DevOps & SRE mindset

---

## 🚀 Future Enhancements

- Dry-run mode (report-only)
- Tag-based exclusions
- Slack / Teams notifications
- Cross-account auditing
- Terraform-based provisioning

---

## 👨‍💻 Author Notes

This project reflects **real-world cloud security challenges** faced by DevOps and SRE teams and demonstrates how automation can proactively reduce risk while maintaining operational efficiency.

> **Security should be enforced by systems — not reminders.**

---

⭐ If this helped you, consider starring the repo and adapting it for your own environments.
