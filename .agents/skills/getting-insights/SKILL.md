---
name: getting-insights
description: Fetches a user's post-run workout insights (LLM-generated summaries) from production DynamoDB. Use when investigating summary quality, checking what feedback the model gave a user after a run, or debugging post-run summary issues.
technologies: [python, aws]
teams: [train-insights]
argument-hint: <user-id> [activity-id]
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get Workout Insights

Fetch a user's post-run workout insights from the production WorkoutInsights DynamoDB table.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_insights.py <user-id> [activity-id]
```

- `user-id`: Required. Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID.
- `activity-id`: Optional. If provided, fetches that specific insight. Otherwise lists 5 most recent.

## Output structure

```json
{
  "userId": "cognito|...",
  "insights": [
    {
      "activityId": "garmin-22292228730",
      "createdAt": "2026-03-25 08:15 UTC",
      "status": "SUCCESS",
      "modelId": "gpt-5.4",
      "promptVersion": "2.1.0",
      "evaluationTraceId": "abc123...",
      "summary": "The full post-run summary text shown to the user...",
      "praises": ["GOOD_EXECUTION"],
      "issues": [],
      "evaluationScore": null,
      "evaluationFeedback": null,
      "freeFormEvaluation": null,
      "errorMessage": null
    }
  ]
}
```

## Display guidance

- Show a summary table first: activity ID, created, status, model, praises count, issues count, evaluated (yes/no)
- For the most recent (or requested) insight, show the full `summary` text
- Show praises and issues as bullet lists
- If evaluation data exists, show score, structured feedback, and free-form notes
- The `evaluationTraceId` can be viewed in Langfuse at `https://cloud.langfuse.com` (project key: `SUMMARY_LANGFUSE_PUBLIC_KEY` in `rb_streamlit/.streamlit/secrets.toml`)
- Cross-reference: the linked activity can be viewed with `/get-activities`, and the workout with `/get-workout`
