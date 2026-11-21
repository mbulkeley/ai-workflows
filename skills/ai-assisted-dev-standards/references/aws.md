# AWS

Applies when a project runs on or deploys to AWS.

## Credentials and IAM

- Never use root credentials. Use IAM users or roles with least-privilege policies.
- Locally, use AWS profiles (`~/.aws/credentials`), never keys in code or in a project's `.env`.
- Deployed services use IAM roles (instance profiles, ECS task roles, Lambda execution roles), not long-lived keys.
- Rotate access keys regularly and review IAM permissions on a schedule.
- Require MFA for console users.

## Secrets management

- Store production secrets in AWS Secrets Manager or Parameter Store.
- Fetch secrets at runtime, never at build time. Never bake them into images.

```python
import json

import boto3


def get_secret(secret_name: str) -> dict:
    client = boto3.client("secretsmanager")
    response = client.get_secret_value(SecretId=secret_name)
    return json.loads(response["SecretString"])
```

## Infrastructure

- Define production infrastructure as code (CDK or Terraform) rather than console clicks.
- Tag every resource with at least `env`, `project` and `owner`.
- Separate development, staging and production by account, or at least by VPC.
- Enable CloudTrail for audit logs and set billing alerts. Leaked credentials often show up first as an unexpected bill.

## Networking

- Put a load balancer in front of services. Don't expose them directly unless necessary.
- Keep databases in private subnets with no public IP.
- Security groups deny everything by default and open only what's needed.
- HTTPS everywhere, with certificates from AWS Certificate Manager.

## Monitoring

- CloudWatch dashboards and alarms for the signals that matter to users: errors, latency and failed jobs.
- Alerts go to a channel someone actually watches, with an owner named.
