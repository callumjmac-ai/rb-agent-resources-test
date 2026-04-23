---
name: getting-workouts
description: Fetches workout data for a user from production DynamoDB. Use when debugging workout scheduling, completion status, linked activities, or comparing planned vs actual performance.
technologies: [python, aws]
teams: [train]
argument-hint: <user-id> [workout-id]
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get Workout

Fetch workout records from the production Workouts DynamoDB table.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_workout.py <user-id> [options]
```

### Options

- `<workout-id>`: Fetch a specific workout by ID
- `--order <order-id>`: Filter to workouts for a specific plan (uses `begins_with`)
- `--limit N`: Number of workouts to fetch (default 10)

### Examples

```bash
# List recent workouts
.../get_workout.py "cognito|abc-123"

# Specific workout
.../get_workout.py "cognito|abc-123" "order-id_plan_week_14_EASY_RUN_0"

# All workouts for active plan
.../get_workout.py "cognito|abc-123" --order "4f04a8d5-6c36-4508-afb1-047a6c9861c2" --limit 20
```

## Output structure

```json
{
  "userId": "cognito|...",
  "workouts": [
    {
      "workoutId": "<orderId>_plan_week_14_EASY_RUN_0",
      "parsed": { "orderId": "...", "week": 14, "runType": "EASY_RUN", "index": 0, "label": "Wk14 EASY_RUN" },
      "workoutName": "W14 Tue Easy - 5km Easy Run with Runna",
      "description": "Full text instructions with paces...",
      "completed": true,
      "skipped": null,
      "scheduledDate": "2026-03-25",
      "linkedActivities": ["garmin-22292228730"],
      "dayOverride": null,
      "weekIndexOverride": null
    }
  ]
}
```

## Workout ID patterns

1. **Plan run workouts**: `<order-id>_plan_week_<N>_<RUN_TYPE>_<index>` (EASY_RUN, LONG_RUN, TEMPO, INTERVALS, HILLS, TAPER_INTERVALS, TIME_TRIAL, RACE)
2. **Plan non-run workouts**: `<order-id>_plan_week_<N>_<TYPE>` — no trailing index (STRETCH, MOBILITY, STRENGTH)
3. **Instant workouts**: `instant-<uuid>`

## Display guidance

- **List mode**: compact table with week, type, name, status (completed/skipped/pending), scheduled date, linked activities
- **Detail mode**: full workout description and steps with target paces
- Note: `ScanIndexForward=False` sorts reverse-lexicographically by workoutId, NOT by date
