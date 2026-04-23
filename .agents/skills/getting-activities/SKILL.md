---
name: getting-activities
description: Fetches a user's recent completed activities from production DynamoDB. Use when investigating user performance, activity history, weather during runs, or debugging post-run summaries.
technologies: [python, aws]
teams: [train]
argument-hint: <user-id> [limit]
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get Activities

Fetch a user's recent activities from the production Activities DynamoDB table.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_activities.py <user-id> [limit]
```

- `user-id`: Required. Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID.
- `limit`: Optional, default 10. Number of recent activities to fetch.

## Output structure

```json
{
  "userId": "cognito|...",
  "activities": [
    {
      "id": "strava-123 | garmin-456 | uuid",
      "name": "Morning Run",
      "type": "RUN | MOBILITY",
      "source": "strava | garmin | in-app",
      "date": "2026-03-25T08:03:58",
      "linked": true,
      "workoutId": "<orderId>_plan_week_14_EASY_RUN_0",
      "distance": "5.02km",
      "duration": "30:31",
      "pace": "6:04/km",
      "avgHR": "127",
      "cadence": "160",
      "elevation": "69",
      "calories": "389",
      "weather": { "description": "...", "temperature": "...", "feelsLike": "...", "windSpeed": "..." }
    }
  ]
}
```

## Display guidance

- Show a summary table: date, name, type, source, distance, duration, pace, HR, linked
- Note duplicate activities from multi-source sync (Strava + Garmin for the same run)
- Run type can be parsed from `workoutId` if linked (e.g. `EASY_RUN`, `LONG_RUN`, `TEMPO`)
- Offer to show raw JSON or detailed splits for any specific activity
