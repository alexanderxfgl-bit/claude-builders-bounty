# n8n Weekly Dev Summary Workflow

Automated GitHub activity summary powered by Claude AI, delivered to Slack/Discord every Friday.

## What It Does

Every Friday at 5PM UTC, this workflow:
1. **Fetches** the past week's commits, closed issues, and merged PRs from a GitHub repo
2. **Formats** the data into a structured summary
3. **Sends to Claude API** to generate an engaging narrative summary
4. **Posts** the summary to Slack and/or Discord

## Setup

### Prerequisites
- n8n instance (self-hosted or cloud)
- GitHub personal access token
- Anthropic API key (Claude)

### Environment Variables
Set these in your n8n credentials/environment:

| Variable | Required | Description |
|----------|----------|-------------|
| `GITHUB_API_URL` | Yes | GitHub API base URL (default: `https://api.github.com`) |
| `GITHUB_REPO` | Yes | Repository in `owner/repo` format |
| `GITHUB_TOKEN` | Yes | GitHub Personal Access Token (stored as HTTP Header Auth credential) |
| `ANTHROPIC_API_KEY` | Yes | Anthropic API key (stored as HTTP Header Auth credential) |
| `CLAUDE_MODEL` | No | Claude model to use (default: `claude-sonnet-4-20250514`) |
| `SLACK_WEBHOOK_URL` | No | Slack incoming webhook URL for posting summaries |
| `DISCORD_WEBHOOK_URL` | No | Discord webhook URL for posting summaries |
| `LANGUAGE` | No | Summary language (default: `EN`) |

### Installation

1. **Import** the workflow:
   - In n8n, go to **Workflows** → **Import from File**
   - Select `n8n-weekly-dev-summary.json`

2. **Configure credentials**:
   - Create a **Header Auth** credential named `GITHUB_TOKEN`:
     - Name: `Authorization`
     - Value: `Bearer YOUR_GITHUB_TOKEN`
   - Create a **Header Auth** credential named `ANTHROPIC_API_KEY`:
     - Name: `x-api-key`
     - Value: `YOUR_ANTHROPIC_API_KEY`

3. **Set environment variables**:
   - In the workflow node settings, set `GITHUB_REPO` to your repo (e.g., `myorg/myproject`)
   - Optionally set `SLACK_WEBHOOK_URL` and/or `DISCORD_WEBHOOK_URL`

4. **Activate** the workflow

### Scheduling

The workflow runs every **Friday at 5PM UTC** by default. To change:

1. Open the **Weekly Trigger** node
2. Edit the cron expression (e.g., `0 9 * * 1` for Monday 9AM)

## Sample Output

```
# Weekly Dev Summary: myorg/myproject
**Period:** 4/7/2026 — 4/14/2026

## This Week in Review

The team shipped 23 commits across 8 contributors, with a focus on
infrastructure improvements and bug fixes...

## Highlights

- 🚀 New CI/CD pipeline reduced build times by 40%
- 🐛 Fixed critical auth bug affecting mobile users
- 📦 Published v2.3.0 with 12 new features

## By the Numbers
- 23 commits, 5 PRs merged, 8 issues closed
- Top contributors: @alice (12), @bob (7), @charlie (4)
```

## Customization

### Add more channels
Duplicate the "Send to Slack" or "Send to Discord" node and modify the webhook URL and payload format.

### Change summary style
Edit the Claude prompt in the "Claude API - Generate Narrative" node to adjust tone, length, or format.

### Track multiple repos
Duplicate the workflow for each repo, or modify the Code node to loop over multiple repositories.

## Architecture

```
Schedule Trigger
    ├── Fetch Weekly Commits ──────┐
    ├── Fetch Closed Issues ───────┼──→ Format Summary Data
    ├── Fetch Merged PRs ──────────┘         │
                                           Claude API
                                              │
                                    ┌─────────┴─────────┐
                                    ↓                   ↓
                              Send to Slack      Send to Discord
```

## Tech Stack
- **n8n** — Workflow orchestration
- **GitHub REST API** — Activity data
- **Claude (Anthropic)** — Narrative generation
- **Slack/Discord Webhooks** — Delivery

## License
MIT
