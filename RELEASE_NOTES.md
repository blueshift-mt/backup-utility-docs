# Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0 Release Notes

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f; font-size: 1.2em;">BREAKING CHANGE: Scheduled Backup Users Must Act</strong>

**If you run backups via Windows Task Scheduler with a parameters file, your backups will stop working after upgrading to v5.0.**

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

Version 5.0 is a ground-up rewrite. The Gooey-based interface has been replaced with a modern Qt application organized into three tabs:

- **Backup tab**: Configure connection, filters, storage, and start on-demand backups
- **Progress tab**: Real-time item-by-item status, active export tracking, and system metrics
- **Schedules tab**: Create, edit, and manage scheduled backups

### Real-Time Progress Monitoring

The Progress tab provides live visibility into running backups:

- Progress bar with item counts (exported / failed / skipped / total)
- Active exports table showing items currently being processed
- Phase indicator (Authenticating, Discovering, Exporting, Retrying, Finishing)
- System metrics (CPU, memory, disk usage)
- Admin/non-admin detection
- Backup and staging folder paths
- Console log output
- Elapsed time

Scheduled backups also report progress to the GUI in real time if the application is open.

### Built-in Schedule Management

Schedules are now created and managed directly in the app. Previous versions required configuring Windows Task Scheduler externally with a parameters file.

- Create daily, weekly, or monthly schedules from the Schedules tab
- Edit, delete, enable/disable, or Run Now from the GUI
- Overlap detection warns when a backup is still running at the next scheduled start
- Schedules run via Windows Task Scheduler under a `CivicLens` folder, even when logged out

### Unified Application

Previous versions used separate executables for different storage targets and modes (od.py, od_s3.py, od_azure.py, od_cleanup.py, sch.py, sch_s3.py, sch_azure.py, sch_cleanup.py). Version 5.0 consolidates everything into a single application. Local storage, S3, Azure, cleanup, and scheduling are all configured from one interface.

### Persistent Settings

All Backup tab configuration is saved automatically between sessions.

### Exclude Item IDs

A new filter allows excluding specific items by ArcGIS item ID (comma-separated).

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

- No more editing spreadsheets or Task Scheduler XML
- Real-time progress monitoring from the GUI
- Email notifications on completion or failure
- Cloud storage configured from the same interface
- Built-in retention policies
- Last run time, status, and warnings visible at a glance

### Cloud Storage

S3 and Azure Blob support has been consolidated into the main application. S3 now supports IAM instance roles and environment credentials in addition to access keys.

### Cleanup Safety

Backup folder retention now validates paths before deletion, blocking drive roots, UNC roots, and protected user directories (Desktop, Documents, etc.). Dry run mode previews deletions before enabling.

### Error Handling

- Authentication and license failures show actionable messages instead of tracebacks
- License validation uses S3 fallback and local cache for resilience
- Temporary export items (`TEMP_FOR_BACKUP`) are cleaned up automatically; cleanup is skipped when no exports occurred
- Duplicate tile package detection skips redundant exports

### Reliability

- Memory-aware downloads with adaptive chunk sizes and garbage collection
- CPU pressure monitoring (memory only in v4.x)
- Disk full detection stops cleanly instead of writing corrupt files
- Consistent Portal URL handling across all export functions

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
