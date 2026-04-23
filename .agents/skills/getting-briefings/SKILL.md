---
name: getting-briefings
description: Fetches a user's recent workout briefings from production DynamoDB. Use when investigating briefing content, weather accuracy, LLM output quality, or tracing issues back to Langfuse.
technologies: [python, aws]
teams: [train-insights]
argument-hint: <user-id> [limit]
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get Briefing

Fetch a user's recent workout briefings from the production WorkoutBriefing DynamoDB table.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_briefing.py <user-id> [limit]
```

- `user-id`: Required. Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID.
- `limit`: Optional, default 5. Number of recent briefings to fetch.

## Output structure

```json
{
  "userId": "cognito|...",
  "briefings": [
    {
      "workoutBriefingId": "<orderId>_plan_week_14_EASY_RUN_0#OUTDOOR",
      "createdAt": "2026-03-25 07:45 UTC",
      "status": "SUCCESS",
      "modelId": "gpt-5-mini",
      "promptVersion": "1.4.2-beta",
      "evaluationTraceId": "abc123...",
      "generationCount": "1",
      "briefing": {
        "focusPoint": { "focus": "RUN_AT_EASY_PACE", "body": "..." },
        "weatherBrief": { "weatherTitle": "...", "weatherAdvice": "..." },
        "planContextBrief": { "title": "...", "advice": "..." },
        "hydrationNutritionBrief": { "hydrationNutritionTitle": "...", "hydrationNutritionAdvice": "..." },
        "routeBrief": null,
        "coachingBrief": null
      }
    }
  ]
}
```

## Display guidance

- Show a summary table first: briefing ID (parsed to readable form), created, status, model
- For the most recent (or requested) briefing, show each section with its content
- Cross-reference weather: if the briefing has weather advice, compare against the user's current forecast (use `/get-weather`) and flag discrepancies
- The `evaluationTraceId` can be viewed in Langfuse at `https://cloud.langfuse.com` (project key: `BRIEF_LANGFUSE_PUBLIC_KEY` in `rb_streamlit/.streamlit/secrets.toml`)
