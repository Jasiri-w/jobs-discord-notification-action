# Discord Workflow Notification

A GitHub Action that sends detailed Discord notifications when workflow jobs fail. This action provides rich, formatted Discord messages containing essential information about the failed workflow, making it easier to track and respond to CI/CD issues.

## Features

- 🔔 Instant Discord notifications for workflow failures
- 📊 Detailed failure information including failed job count and names
- 🔗 Direct links to workflow runs and commits
- 💅 Rich Discord embeds with formatted content
- 🎨 Color-coded messages for better visibility
- ⏰ Timestamp information for tracking
- 📌 Branch and repository context

## Installation

To use this action in your workflow, add the following step to your `.github/workflows/` YAML file:

```yaml
- uses: lacherogwu/failed-jobs-discord-notification-action@v1
  with:
    discord_webhook_url: ${{ secrets.DISCORD_WEBHOOK_URL }}
    needs_json: ${{ toJSON(needs) }}
```

## Usage

This action requires two inputs:

| Input                 | Description                                       | Required |
| --------------------- | ------------------------------------------------- | -------- |
| `discord_webhook_url` | Discord webhook URL for sending notifications     | Yes      |
| `needs_json`          | JSON string containing the workflow needs context | Yes      |

### Prerequisites

1. Create a Discord webhook:

   - Go to your Discord server
   - Select a channel
   - Edit Channel → Integrations → Create Webhook
   - Copy the webhook URL

2. Add the webhook URL to your repository secrets:
   - Go to your repository Settings
   - Select Secrets and variables → Actions
   - Create a new secret named `DISCORD_WEBHOOK_URL`
   - Paste your Discord webhook URL

## Example

Here's a complete workflow example showing how to implement the Discord notification:

```yaml
name: CI/CD Pipeline

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: |
          # Your test commands here
          exit 1

  notify:
    needs: [test]
    runs-on: ubuntu-latest
    if: failure()
    steps:
      - uses: lacherogwu/failed-jobs-discord-notification-action@v1
        with:
          discord_webhook_url: ${{ secrets.DISCORD_WEBHOOK_URL }}
          needs_json: ${{ toJSON(needs) }}
```

### Discord Notification Format

The notification will include:

- Workflow name
- Repository name
- Branch name
- Number of failed jobs
- List of failed job names
- Direct link to the workflow run
- Commit hash and link
- Timestamp of the failure

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

LacheRo`

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

If you encounter any problems or have suggestions, please file an issue in the GitHub repository.
