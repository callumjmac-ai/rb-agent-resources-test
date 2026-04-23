---
name: getting-user-profile
description: Fetches a Runna user's profile from production DynamoDB. Use when investigating user issues, checking subscription status, or understanding a user's context. PII is redacted by default.
technologies: [python, aws]
argument-hint: <user-id>
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get User

Fetch a user's profile from production DynamoDB. PII (name, DOB, location) is redacted by default.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_user.py <user-id>
```

Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID (tries cognito, then apple).
