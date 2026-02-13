# Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0 Release Notes

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f; font-size: 1.2em;">BREAKING CHANGE: Scheduled Backup Users Must Act</strong>

**If you run backups via Windows Task Scheduler with a parameters file, your backups will stop working after upgrading to v5.0 unless you do the following:**

The application no longer auto-detects parameters files on launch. Double-clicking the executable now always opens the GUI. To restore headless operation, you must add `--file` to your scheduled task arguments.

**Immediate fix:**

1. Open **Task Scheduler** and find your backup task
2. Right-click the task and select **Properties**
3. Go to the **Actions** tab and click **Edit**
4. In the **Add arguments** field, add: `--file "C:\path\to\parameters.xlsx"`
5. Click **OK** to save

**Parameters file support is deprecated and will be removed in a future release.** We recommend migrating to the built-in Schedules tab, which is easier to set up, supports OAuth/SSO, provides real-time progress monitoring, and stores credentials securely. See [Migrating from Parameters Files](#migrating-from-parameters-files) for details.

</div>

---

## What's New

### Redesigned Interface

Version 5.0 features a completely redesigned interface organized into three tabs:

- **Backup tab**: Configure connection, filters, storage, and start on-demand backups
- **Progress tab**: Real-time item-by-item status, active export tracking, and system metrics
- **Schedules tab**: Create, edit, and manage scheduled backups

### Real-Time Progress Monitoring

The Progress tab shows live status for running backups:

- Progress bar with item counts (exported / failed / skipped / total)
- Active exports table showing each item currently being processed, its type, and the current API response status (e.g., Pending, ExportingData, ExportingAttachments, Completed) for Feature Service exports
- Phase indicator tracking each stage of the backup: Authenticating, Discovering Items, Exporting, Retry Pass 1–11, and Finishing
- System metrics (CPU, memory, disk usage)
- Admin/non-admin detection with a warning if running without administrator privileges
- Backup and staging folder paths
- Console log with per-item export results as they complete
- Elapsed time

Open the app during a scheduled backup to monitor its progress in real time.

### Built-in Schedule Management

Schedules are now created and managed directly in the app. Previous versions required configuring Windows Task Scheduler externally with a parameters file.

- Create daily, weekly, or monthly schedules from the Schedules tab
- Edit, delete, enable/disable, or Run Now from the GUI
- Overlap detection warns when a backup is still running at the next scheduled start
- Schedules run via Windows Task Scheduler under a `CivicLens` folder, even when logged out

### Unified Application

Previous versions used separate executables for different storage targets and modes. Version 5.0 consolidates local storage, S3, Azure, cleanup, and scheduling into a single application.

### Persistent Settings

All Backup tab configuration is saved automatically between sessions.

### Exclude Item IDs

Exclude specific items by ArcGIS item ID (comma-separated).

---

## Improvements

### Migrating from Parameters Files

As noted in the [breaking change notice](#-breaking-change-scheduled-backup-users-must-act) above, parameters files are no longer auto-detected. Pass `--file` to continue using them, or migrate to the Schedules tab.

> **Parameters file support will be removed in a future release.** Migrate to the built-in Schedules tab as soon as possible.

The `--file` flag accepts both formats:

    BackupUtility.exe --file "C:\Backups\parameters.xlsx"
    BackupUtility.exe --file "C:\Backups\parameters.csv"

**To update an existing Task Scheduler task:**

1. Open **Task Scheduler** and find your backup task
2. Right-click the task and select **Properties**
3. Go to the **Actions** tab and click **Edit**
4. In the **Add arguments** field, add: `--file "C:\path\to\parameters.xlsx"`
5. Click **OK** to save

**Why migrate to the Schedules tab:**

- No more editing spreadsheets or Task Scheduler tasks
- Real-time progress monitoring from the GUI
- Cloud storage configured from the same interface
- Last run time, status, and warnings visible at a glance

### Cloud Storage

S3 and Azure Blob are now built in. S3 also supports IAM instance roles and environment credentials in addition to access keys.

### Cleanup Safety

Retention validates paths before deletion, blocking drive roots, UNC roots, and protected directories (Desktop, Documents, etc.). Dry run previews what would be deleted.

### Error Handling

- Authentication and license failures show actionable messages instead of tracebacks
- License validation uses S3 fallback and local cache for resilience

### Reliability

- Memory-aware downloads with adaptive chunk sizes and garbage collection
- CPU pressure monitoring (memory only in v4.x)
- Disk full detection stops cleanly instead of writing corrupt files

---

## Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended)
- Network access to ArcGIS Online or Portal for ArcGIS

---

## Support

- Docs: https://docs.civiclens.com
- Email: support@civiclens.com
- Downloads: https://civiclens.com/downloads
