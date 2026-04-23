# Runna Engineering Team Structure

> Gives agents visibility into team ownership. Reference this when determining which team owns a repo or skill, or when filtering resources by team.
>
> Live doc: https://www.notion.so/runna/Tech-Org-e038fe1eec694ec8bd203a4436452bd4

## Product Teams and Squads

| Team | Squad | Parent slug | Squad slug |
|------|-------|-------------|------------|
| **App** | Core | `app` | `app-core` |
| | Activities | `app` | `app-activities` |
| | Strength & Community | `app` | `app-strength` |
| | Recording | `app` | `app-recording` |
| **Growth** | Stunna (Acquisition) | `growth` | `growth-acquisition` |
| | Stunna (Monetisation) | `growth` | `growth-monetisation` |
| | GoPro | `growth` | `growth-gopro` |
| **Train** | Engine | `train` | `train-engine` |
| | Adaptations | `train` | `train-adaptations` |
| | Insights | `train` | `train-insights` |
| | Performance | `train` | `train-performance` |
| **Special Projects** | Race Discovery | `special-projects` | `race-discovery` |
| | RISE | `special-projects` | `rise` |
| **Data & Platform** | Platform | `data-platform` | `platform` |
| | Data | `data-platform` | `data` |

## How team filtering works

Skills can declare `teams:` in their frontmatter. Consumers declare their teams in `.agents/config.yml`.

A consumer should list **all teams it belongs to, including the parent team**. For example, a repo owned by the Insights squad would declare:

```yaml
teams: [train, train-insights]
```

A skill tagged `teams: [train]` syncs to all Train repos. A skill tagged `teams: [train-engine]` only syncs to Engine squad repos.
