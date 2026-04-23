---
name: getting-weather
description: Fetches a user's weather forecast from production DynamoDB. Use when debugging briefing weather accuracy, checking conditions for an upcoming workout, or investigating weather-related issues.
technologies: [python, aws]
teams: [train-insights]
argument-hint: <user-id> [--date YYYY-MM-DD]
allowed-tools: Bash(PYTHONPATH=* .venv/bin/python *)
---

# Get Weather

Fetch a user's current weather forecast from their profile in production DynamoDB. Location is redacted to city-level.

## Usage

```bash
PYTHONPATH=. .venv/bin/python .claude/scripts/get_weather.py <user-id> [--date YYYY-MM-DD]
```

- `user-id`: Required. Accepts `cognito|<uuid>`, `apple|<id>`, or bare UUID.
- `--date`: Optional. Filter forecast to a specific date.

## Output structure

```json
{
  "userId": "cognito|...",
  "weather": {
    "source": "openweather",
    "city": "London",
    "timestamp": "2026-03-26 06:00 UTC",
    "location": { "lat": "51.5", "lng": "-0.1", "source": "gps" },
    "current": { "date": "...", "status": "...", "statusDescription": "...", "temperature": "...", "feelsLike": "...", "windSpeed": "...", "precipitation": "...", "snow": "...", "humidity": "..." },
    "forecast": [ "..." ]
  }
}
```

## Display guidance

- Show current conditions prominently
- Show forecast as a table: date, conditions, temp, feels like, wind, precip
- If `--date` used, show only that day's forecast
- Cross-reference with `/get-briefing` weather advice to flag discrepancies
