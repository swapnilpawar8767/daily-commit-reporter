# daily-commit-reporter

This GitHub Actions workflow collects commits made across multiple repositories within the last 24 hours and sends a daily summary report to a Microsoft Teams channel using a webhook.

---
## 📁 Repository Structure
>commit-monitor/
>
> ├── repos.txt
>
> └── .github/workflows/daily-commit-report.yaml
---

## 🚀 Features

- ✅ Collects commits from multiple public or private repositories.
- ✅ Shows commit title, author, and clickable link.
- ✅ Sends the summary to Microsoft Teams.
- ✅ Runs daily via cron or manually on-demand.

---

## 🔐 Requirements

### 1. GitHub Personal Access Token (PAT)

To access multiple private or public repositories from a central repo:

- Go to [GitHub → Settings → Developer settings → Personal access tokens](https://github.com/settings/tokens).
- Click **"Generate new token (fine-grained)"**
- Set **Resource owner** to your GitHub account.
- Set **Expiration** to your preferred duration.
- Under **Repository Access**, select **"Only select repositories"** → Choose the ones you want to monitor.
- Under **Permissions**, enable the following:

Contents: Read-only
Metadata: Read-only

- Click **Generate Token**, then copy it.

> 🔐 **Important**: Store this token in your repo secrets as `GH_TOKEN`.

---

### 2. Microsoft Teams Webhook

To receive commit reports in a Teams channel:

- Open Microsoft Teams.
- Go to the channel where you want the messages.
- Click `...` (More options) → **Connectors**
- Search for **Incoming Webhook** → Click **Configure**
- Give it a name like `GitHub Commit Bot` and upload an icon if you like.
- Copy the generated **Webhook URL**.

> 🔐 **Important**: Store this in your repo secrets as `TEAMS_WEBHOOK_URL`.

---

## 📄 Configuration Files

### `repos.txt`

This file lists all the GitHub repositories to monitor, one per line:

your-org/repo-1
your-org/repo-2
another-org/repo-3

> ✅ You can monitor both public and private repositories (if your PAT has access).

---

## ⚙️ Workflow Details

### `.github/workflows/daily-commit-report.yaml`

- Triggered **daily at 6 AM IST** (`cron: '0 6 * * *'`)
- Also supports **manual trigger** (`workflow_dispatch`)
- Reads `repos.txt`, fetches last 24 hours' commits, formats, and sends to Teams

---

## ✅ Setup Steps Summary

1. **Create this repo** (e.g., `commit-monitor`)
2. **Add `repos.txt`** with repo names
3. **Add the workflow** at `.github/workflows/daily-commit-report.yaml`
4. **Generate PAT** and add it as `GH_TOKEN` in repo secrets
5. **Create Teams webhook**, add it as `TEAMS_WEBHOOK_URL` in secrets
6. Workflow will run daily or on manual trigger

---

## 📬 Sample Output in Teams

📊 GitHub Daily Commit Report (2025-07-29)

your-org/repo-1
Fix login bug [#commitlink] by John Doe

Add unit tests for service [#commitlink] by Jane Smith

your-org/repo-2
No commits in the last 24 hours.

---

## ❓ Troubleshooting

- If the report shows **"No commits"** but you're sure there were some:
  - Check if your PAT has access to the repo.
  - Ensure the repo name is correct in `repos.txt`.
  - Confirm commit timestamps are within UTC last 24 hours.

---
