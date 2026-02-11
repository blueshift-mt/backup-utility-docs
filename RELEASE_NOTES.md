# Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0 Release Notes

## What's New

### Redesigned Interface

The application has been completely rebuilt with a modern tabbed interface:

- **Backup tab** — Configure and start backups
- **Monitor tab** — Watch progress in real-time with item-by-item status
- **Schedules tab** — Set up automatic backups without editing or managing config files

### Secure Password Storage

Your passwords are now optionally stored securely in Windows Credential Manager for both on-demand and scheduled backups. or SSO organizations, OAuth tokens are also stored securely so scheduled backups can run unattended without requiring built-in user authentication (scheduled backups now work with SSO-only and MFA-enforced logins).

### Built-in Scheduling

Set up daily, weekly, or monthly backups directly in the app. No more editing XML files or using Task Scheduler manually. Schedules run even when you're logged out.

### Migrating from Parameters Files

Parameters files (parameters.xlsx or parameters.csv) are still supported, but we strongly recommend migrating to the built-in Schedules tab as soon as possible. Parameters file scheduling is deprecated and will be removed in a future release.

Benefits of the Schedules tab:

- **Easier management** — No more editing spreadsheets
- **OAuth/SSO support** — Scheduled backups work with single sign-on
- **Progress monitoring** — Watch backups run in real-time
- **Secure credentials** — Passwords stored in Windows Credential Manager

**Important:** If a parameters.xlsx or parameters.csv file is in the same folder as the exe, the application will automatically run a headless backup instead of opening the GUI. To open the GUI, move the exe to a folder without a parameters file. This behavior will be removed in a future release when parameters file support is discontinued.

### Settings That Stick

Your configuration is saved automatically. Close the app, reopen it tomorrow — everything is right where you left it.

### Proxy Support

Configure an HTTP/HTTPS proxy in the Advanced section if your network requires it.

---

## Other Improvements

- Admin/non-admin status shown on Progress tab
- CPU/RAM/ available disk now displayed in app console
- Online documentation at docs.civiclens.com
- Improved error messages
- Cleaner Results.txt formatting and logging improvements

---

## Support

- Docs: https://docs.civiclens.com
- Email: support@civiclens.com
