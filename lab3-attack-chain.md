# Lab 3 — SSRF Attack Chain & Remediation

## What We Did (Attack Chain)

1. Found SSRF vulnerability in Link Preview web app (no URL filtering)
2. Sent IMDS URL through app → got EC2 IAM role credentials
3. Loaded stolen credentials into Pacu
4. Enumerated 256 permissions (S3ReadOnly + IAMFullAccess)
5. Created backdoor IAM user with AdministratorAccess
6. Reset RDS master password using IAM permissions
7. Connected to RDS → dumped 100 customer records (names, emails, card numbers)

**One SSRF vulnerability = full AWS account takeover + data breach**

---

## Remediation

| Vulnerability | Fix |
|---|---|
| SSRF in app | Validate URLs, block internal IP ranges (169.254.x.x) |
| IMDSv1 enabled | Enforce IMDSv2 (HttpTokens: required) |
| IMDS hop limit | Set hop limit to 1 |
| Overprivileged IAM role | Remove IAMFullAccess, apply least privilege |
| RDS publicly accessible | Disable public access, move to private subnet |
| RDS unencrypted | Enable storage encryption |
| Hardcoded DB credentials | Use AWS Secrets Manager |
