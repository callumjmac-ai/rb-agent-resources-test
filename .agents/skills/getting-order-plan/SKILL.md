---
name: getting-order-plan
description: Fetches and decompresses a user's training plan from production DynamoDB OrderPlans table. Use when debugging plan structure, checking workout scheduling, or understanding a user's training programme.
technologies: [python, aws]
teams: [train-engine]
argument-hint: <user-id> [order-id]
allowed-tools: Bash(PYTHONPATH=* */.venv/bin/python *)
---

# Get Order Plan

Fetch and decompress a user's training plan from the production OrderPlans DynamoDB table.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_order_plan.py <user-id> [order-id]
```

- `user-id`: Required. Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID.
- `order-id`: Optional. If omitted, fetches the user's `activeOrder` from the Users table.

## Output structure

```json
{
  "userId": "cognito|...",
  "plan": {
    "orderId": "...",
    "displayName": "Half Marathon - Intermediate",
    "raceDistance": "HALF_MARATHON",
    "startDate": "2025-12-29",
    "raceDate": "2026-04-19",
    "nWeeks": 16,
    "currentWeek": 13,
    "weeks": {
      "Week N": {
        "totalDistanceFormatted": "42.5km",
        "weekType": "NORMAL",
        "workouts": {
          "RUN_TYPE_0": { "runTitle": "...", "runTypeDisplay": "...", "distanceFormatted": "...", "scheduledDate": "..." }
        }
      }
    }
  }
}
```

## Display guidance

- Show plan overview: name, race distance, race date, current week / total weeks
- Show current week's workouts in a table
- Show weekly distance progression if asked
- Cross-reference with `/get-workout` for completion data
