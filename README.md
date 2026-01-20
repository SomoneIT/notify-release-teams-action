# Notify Release to Teams

A GitHub Action that sends a Microsoft Teams notification when a new release is published.

![License](https://img.shields.io/badge/license-MIT-blue.svg)

## Features

- Automatically notify your team on Microsoft Teams when a new release is published
- Includes release notes in the notification
- Customizable theme color for the notification card
- Direct link to view the release on GitHub

## Usage

### Prerequisites

1. Create a Microsoft Teams Incoming Webhook:
   - In Microsoft Teams, go to the channel where you want notifications
   - Click on the three dots (`...`) next to the channel name
   - Select **Connectors** (or **Workflows** for newer Teams)
   - Search for **Incoming Webhook** and configure it
   - Copy the webhook URL

2. Store the webhook URL as a GitHub secret (e.g., `TEAMS_WEBHOOK_URL`)

### Basic Example

```yaml
name: Notify Teams on Release

on:
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Send Teams Notification
        uses: SomoneIT/notify-release-teams-action@v1
        with:
          webhook-url: ${{ secrets.TEAMS_WEBHOOK_URL }}
```

### Advanced Example

```yaml
name: Notify Teams on Release

on:
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Send Teams Notification
        uses: SomoneIT/notify-release-teams-action@v1
        with:
          webhook-url: ${{ secrets.TEAMS_WEBHOOK_URL }}
          version: ${{ github.event.release.tag_name }}
          repository: ${{ github.repository }}
          theme-color: "00FF00"
```

### Manual Trigger Example

You can also trigger notifications manually for any version:

```yaml
name: Manual Teams Notification

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'Version to notify about'
        required: true

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Send Teams Notification
        uses: SomoneIT/notify-release-teams-action@v1
        with:
          webhook-url: ${{ secrets.TEAMS_WEBHOOK_URL }}
          version: ${{ github.event.inputs.version }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `webhook-url` | Microsoft Teams webhook URL | **Yes** | - |
| `version` | Version to notify about | No | `github.ref_name` (for releases) |
| `repository` | Repository name | No | `github.repository` |
| `theme-color` | Card theme color (hex without `#`) | No | `0076D7` |

## Outputs

| Output | Description |
|--------|-------------|
| `release-url` | URL to the release on GitHub |

### Using Outputs

```yaml
- name: Send Teams Notification
  id: teams-notify
  uses: SomoneIT/notify-release-teams-action@v1
  with:
    webhook-url: ${{ secrets.TEAMS_WEBHOOK_URL }}

- name: Print Release URL
  run: echo "Release URL: ${{ steps.teams-notify.outputs.release-url }}"
```

## Notification Preview

The notification sent to Microsoft Teams includes:

- **Title**: "A new {repository-name} version {version} has been released!"
- **Version**: The release version number
- **Release Notes**: The body/description of the GitHub release
- **Action Button**: "View Release" - links directly to the GitHub release page

## Theme Colors

The `theme-color` input accepts any hex color code (without the `#`). Here are some examples:

| Color | Hex Code |
|-------|----------|
| Blue (default) | `0076D7` |
| Green | `00FF00` |
| Red | `FF0000` |
| Orange | `FFA500` |
| Purple | `800080` |

## Permissions

This action uses the `github.token` to fetch release information. No additional permissions are required for public repositories. For private repositories, ensure the workflow has `contents: read` permission.

```yaml
permissions:
  contents: read
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Author

[SomoneIT](https://github.com/SomoneIT)
