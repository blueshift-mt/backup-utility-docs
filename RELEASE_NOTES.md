# Backup Utility v5.0 Release Notes

## What's New

### Redesigned Interface

Version 5.0 features a completely rebuilt GUI using PySide6. The new interface consolidates all backup functionality into a single application with tabbed navigation:

- **Backup tab** — Configure and run on-demand backups
- **Monitor tab** — Track active backups with real-time progress, item status, and console output
- **Schedules tab** — Create and manage scheduled backups without leaving the app

### Secure Credential Storage

Passwords and OAuth tokens are now stored in **Windows Credential Manager** instead of config files:

- ArcGIS passwords stored under service name `CIVICLENS` or your org URL
- OAuth refresh tokens stored per-schedule for SSO/SAML organizations
- Enter `wcm` as password to use stored credentials
- No sensitive data written to disk in plain text

### Built-in Scheduler

Create and manage Windows scheduled tasks directly from the application:

- Daily, weekly, or monthly schedules
- Run backups while logged out or screen locked (requires Windows credentials)
- Enable/disable schedules without deleting them
- "Run Now" button for immediate execution
- Automatic Windows Task Scheduler integration

### Session Persistence

Your settings are saved automatically between sessions:

- Organization URL, username, save path
- Selected options (item types, scan type, thread count, timeout)
- Filter settings (tags, owner, groups, folders)
- Cloud storage configuration
- Window size and position

### OAuth/SSO Support

Browser-based authentication for organizations using SAML, OAuth, or MFA:

- On-demand: Check SSO/OAuth box, click Start Backup, sign in via browser
- Scheduled: Authorize once, refresh tokens handle subsequent runs
- Tokens refresh automatically as long as backups run regularly
- Re-authorization prompt when organization URL changes

### Cloud Storage

Upload backups directly to cloud storage after local staging:

- **Amazon S3** — Supports IAM credentials or instance roles
- **Azure Blob Storage** — Connection string or SAS token authentication
- Optional: Delete local copy after successful upload

### Proxy Support

HTTP/HTTPS proxy configuration for enterprise networks. All ArcGIS API connections route through the configured proxy.

---

## Other Changes

- **Admin indicator** — Progress tab shows whether running as org admin or non-admin
- **CPU display** — Now shows normalized CPU percentage matching Task Manager
- **Documentation** — Online docs at docs.civiclens.com replace PDF user guide
- **Error messages** — Centralized for consistency and easier customization
- **Results.txt** — Improved formatting with better alignment and failure details

---

## Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended)
- Network access to ArcGIS Online or Portal for ArcGIS

---

## Support

- Documentation: https://docs.civiclens.com
- Email: support@civiclens.com
