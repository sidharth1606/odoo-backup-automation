# Odoo Backup Automation

Automated daily backup workflow for Odoo.sh instances to GitHub and OneDrive.

## Features

- **Scheduled backups**: Automatically backs up your Odoo database daily at 5:30 AM UTC
- **SSH access**: Securely downloads backups from Odoo.sh using SSH
- **Git LFS**: Stores large backup files efficiently using Git LFS
- **OneDrive sync**: Automatically uploads backups to OneDrive for redundancy
- **Manual trigger**: Run backups on-demand using `workflow_dispatch`

## Setup

### 1. Add GitHub Secrets

Add the following secrets to your repository (Settings → Secrets and variables → Actions):

| Secret | Description |
|--------|-------------|
| `ODOO_SSH_KEY` | Your private SSH key for Odoo.sh access |
| `ODOO_SSH_HOST` | Odoo.sh SSH host (e.g., `user@server.odoo.sh`) |
| `ODOO_BACKUP_PATH` | Path to the backup file on the server (e.g., `/path/to/backup.sql.gz`) |
| `MS_CLIENT_ID` | Microsoft Azure Client ID for OneDrive access |
| `MS_REFRESH_TOKEN` | Microsoft refresh token for OneDrive API |
| `ONEDRIVE_FOLDER` | OneDrive folder name to store backups (e.g., `OdooBackups`) |

### 2. Enable Git LFS

```bash
git lfs install
git lfs track "*.sql.gz"
git add .gitattributes
git commit -m "Enable Git LFS for backup files"
git push
```

### 3. Configure SSH Access

Ensure your SSH key is added to your Odoo.sh account and has proper permissions.

## How It Works

1. **Checkout**: Pulls the latest repository code
2. **SSH Setup**: Configures SSH authentication
3. **Download**: Fetches the backup from your Odoo.sh instance
4. **Validate**: Verifies the backup file is not empty
5. **Git LFS**: Tracks the backup with Git LFS
6. **Commit**: Commits the backup to the repository
7. **OneDrive**: Uploads the backup to OneDrive for additional redundancy

## OneDrive Setup

To set up OneDrive integration:

1. Register an app in [Azure Portal](https://portal.azure.com)
2. Create a client secret
3. Get a refresh token using OAuth 2.0 flow
4. Add the credentials to GitHub Secrets

## Manual Trigger

To run the backup workflow manually:

1. Go to **Actions** tab
2. Select **Odoo Daily Backup** workflow
3. Click **Run workflow**

## Notes

- Backups run daily at 5:30 AM UTC
- Failed backups will exit with an error and prevent pushing
- Ensure Git LFS is properly configured to handle large files
- Keep your secrets secure and rotate them periodically

## License

MIT
