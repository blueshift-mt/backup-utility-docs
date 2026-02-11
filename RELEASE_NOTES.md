# Backup Utility v5.0 Release Notes

## What's New

### Redesigned Interface

The application has been completely rebuilt with a modern tabbed interface:

- **Backup tab** — Configure and start backups
- **Monitor tab** — Watch progress in real-time with item-by-item status
- **Schedules tab** — Set up automatic backups without editing config files

### Secure Password Storage

Your passwords are now stored securely in Windows Credential Manager. Check "Remember Password" and you won't need to enter it again. For SSO organizations, OAuth tokens are also stored securely so scheduled backups can run unattended.

### Built-in Scheduling

Set up daily, weekly, or monthly backups directly in the app. No more editing XML files or using Task Scheduler manually. Schedules run even when you're logged out.

### Settings That Stick

Your configuration is saved automatically. Close the app, reopen it tomorrow — everything is right where you left it.

### SSO/OAuth Support

For organizations using single sign-on:
- Check the SSO/OAuth box
- Click Start Backup (or Authorize for schedules)
- Sign in through your browser
- Done

### Cloud Storage

Back up directly to Amazon S3 or Azure Blob Storage. Files are staged locally first, then uploaded automatically.

### Proxy Support

Configure an HTTP/HTTPS proxy in the Advanced section if your network requires it.

---

## Other Improvements

- Admin/non-admin status shown on Progress tab
- CPU usage now displays correctly (matches Task Manager)
- Online documentation at docs.civiclens.com
- Better error messages
- Cleaner Results.txt formatting

---

## Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM (8 GB recommended)

---

## Support

- Docs: https://docs.civiclens.com
- Email: support@civiclens.com
