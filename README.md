# Discord Workflow Notification for GitHub Actions

A GitHub Action that sends rich, detailed Discord notifications for your GitHub workflow runs. It tracks **all job statuses**, **wakes up hibernating apps** (if applicable), and formats the information cleanly into a Discord embed for instant visibility.

## Features

- 🔔 Instant Discord notifications for all workflow runs
- 🧩 Shows **status of every job** (✅ success / ❌ failure)
- 🚀 Detects and displays **wake-up attempts** if your app was hibernating
- 🔗 Direct links to workflow runs and commits
- 🎨 Color-coded messages:
  - Green 🟩 if all jobs succeed
  - Red 🟥 if any job fails
- 📌 Displays repository and branch context
- ⏰ Includes timestamp for easy tracking
- 💬 Clean and professional message formatting

## Installation

Add this to your `.github/workflows/*.yml`:

```yaml
- uses: your-username/jobs-discord-notification-action@your-branch-or-tag
  with:
    discord_webhook_url: ${{ secrets.DISCORD_WEBHOOK_URL }}
    needs_json: ${{ toJSON(needs) }}
    wake_up_log: ${{ needs.probe_deployed_app.outputs.wake_up_log }}
```

> Replace `your-username` and `your-branch-or-tag` accordingly.

## Inputs

| Input                | Description                                                | Required |
| -------------------- | ----------------------------------------------------------- | -------- |
| `discord_webhook_url` | Discord webhook URL for sending the notification            | Yes      |
| `needs_json`         | JSON string containing status info about all workflow jobs  | Yes      |
| `wake_up_log`        | (Optional) Output from app probing if a wake-up was attempted | No       |

## Setup Prerequisites

1. **Create a Discord webhook:**
   - Go to your Discord server
   - Open channel settings → Integrations → Webhooks → New Webhook
   - Copy the webhook URL

2. **Save the webhook URL to GitHub Secrets:**
   - Go to GitHub → Repository → Settings → Secrets and variables → Actions
   - Create a new secret named `DISCORD_WEBHOOK_URL`
   - Paste the copied Discord webhook URL

## Example Workflow

```yaml
name: Keep App Alive and Notify

on:
  schedule:
    - cron: '0 */10 * * *' # Every 10 hours
  workflow_dispatch:

jobs:
  probe_deployed_app:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Probe Deployed App
        uses: ./probe-action
        id: probe

  notify:
    needs: [probe_deployed_app]
    runs-on: ubuntu-latest
    steps:
      - uses: your-username/jobs-discord-notification-action@your-branch-or-tag
        with:
          discord_webhook_url: ${{ secrets.DISCORD_WEBHOOK_URL }}
          needs_json: ${{ toJSON(needs) }}
          wake_up_log: ${{ needs.probe_deployed_app.outputs.wake_up_log }}
```

## Discord Notification Example

- Repository: `user/repo`
- Branch: `main`
- Jobs:
  - ✅ `test`
  - ❌ `deploy`
- Wake Up Log: `App hibernating. Attempting to wake up!`
- Links:
  - Workflow Run
  - Commit

## License

This project is licensed under the [MIT License](LICENSE).

## Author

Maintained by **Jasiri Wa-Kyendo**.
