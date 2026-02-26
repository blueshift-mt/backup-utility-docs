# Release Notes

<a id="breaking-change-scheduled-backup-users-must-act-v50"></a>

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f; font-size: 1.2em;">BREAKING CHANGE: Scheduled Backup Users Must Act (v5.0+)</strong>

**If you run backups via Windows Task Scheduler with a parameters file, your backups will stop working after upgrading to v5.0+ unless you do the following:**

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

## Version 5.1.7

*Includes all features from [5.1.6](#version-516) and earlier.*

- **Adaptive Download Retries** - Large file downloads that keep breaking mid-stream are no longer abandoned after 10 attempts if the download is making real progress. Each attempt that writes more than 1 MB of data extends the retry limit automatically (up to 50 total attempts). This prevents unnecessary fallback to slower export methods for multi-gigabyte Feature Services where the connection drops but resume is working.

- **Duplicate File Prevention** - When a Feature Service replica download fails and the item is re-exported via the item export API, the partial file from the failed replica is now cleaned up. Previously, both files could end up in the backup folder with slightly different filenames, resulting in duplicate files and invalid zip warnings.

- **OAuth Token Rotation Persistence** - When ArcGIS Online rotates an OAuth refresh token during a scheduled backup, the new token is now persisted to both Windows Credential Manager (for interactive/impersonated service accounts) and DPAPI machine-scope storage (for SYSTEM/gMSA accounts). Previously, the rotated token was only used in-memory, causing the next scheduled run to fail with an expired token.

- **Cross-User Schedule Fix** - Creating a scheduled backup as a different Windows user (e.g., a service account) no longer fails when the current user is not an administrator. Previous versions required admin privileges to load the run-as user's profile for credential storage. Non-admin users now store credentials via DPAPI machine-scope encryption automatically.

- **Cloud Backup Location in Results** - Results.txt, email notifications, and console output now include the full S3 or Azure path where the backup was uploaded (e.g., `s3://bucket/folder/`).

- **Scheduling Reliability** - Improved error diagnostics for scheduled task creation failures, credential validation hardening, and DPAPI round-trip testing in the Copy Diagnostics output.

---

## Version 5.1.6

*Includes all features from [5.1.5](#version-515) and earlier.*

- **Azure Default Credentials** - Azure Blob Storage now supports authentication via Azure CLI, Managed Identity, or environment variables. Enter just an Account Name (no connection string needed) and the application uses Azure's default credential chain. Works in Backup, Restore, and Scheduled Backups.

- **HFLV Feature Data Export** - New option to export Hosted Feature Layer View data as File Geodatabase. Previously, views were backed up as JSON definitions only. Enable "HFLV Feature Data" in Advanced Options (or in the schedule editor) to include the underlying feature data.

- **Schedule Diagnostics** - New "Copy Diagnostics" button in the Schedules tab collects OS info, task status, credential health (DPAPI round-trip test), file permissions, and recent error logs into a single clipboard-ready block for support. Also available in the task creation failure dialog alongside the XML export option. Scheduled backups that crash now include account identity, DPAPI status, and file access checks in the crash log.

- **Scheduling Improvements** - Info buttons throughout the schedule editor now link directly to relevant documentation. SMTP servers that don't require a password no longer show a misleading "notifications will fail" warning. Docs links added to OAuth, Windows User, Storage, and other schedule settings.

- **Credential Fallback for Restore and Find & Replace** - The Restore and Find & Replace tabs now fall back to credentials stored by the Backup tab when no tab-specific password is saved, so you don't need to re-enter your password after backing up.

- **Item Tagging Fix** - The "Tag Backed-Up Items" option now works correctly. Items are tagged with `last_backup_YYYYMMDD_HHMMSS` after successful export, or `_PARTIAL` for items that exported in a degraded format (SHP-only, CSV-only, missing attachments).

- **Portal URL Fix** - Portal URLs ending in `/home` (commonly copied from the browser address bar) are now normalized correctly during license lookup. Previously this caused a "not licensed" error.

- **StoryMap Restore Fix** - Creating a StoryMap as new no longer fails when the server auto-creates a phantom `published_data.json` resource. The application detects the conflict and retries automatically.

---

## Version 5.1.5

*Includes all features from [5.1.4](#version-514) and earlier.*

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f; font-size: 1.2em;">Action Required: Schedules Running Under a Service Account</strong>

**If any of your scheduled backups run under a Windows service account (a different user than the one who created the schedule), you need to open the application and complete the update dialog to fix credential storage.**

Previous versions could silently fail to store credentials for the service account, causing authentication failures on the next run. This version fixes the underlying issue.

**To fix:** Open the application after updating. An update dialog will appear listing affected tasks. Enter your **Windows username** and **password** and click **Update Tasks**. The application recreates the scheduled tasks and replicates all stored credentials (portal password, OAuth tokens, S3/Azure keys, SMTP password) to the service account automatically.

If you skip the dialog, you can fix schedules individually:

1. Open the application and go to the **Schedules** tab
2. Click **Edit** on each schedule that runs under a service account
3. Re-enter the **Windows username** and **password** and click **Save**
4. No other settings need to change

If your schedules run under the same account that created them, no action is needed.

</div>

- **Enterprise Login Auto-Fallback** - Built-in ArcGIS accounts on organizations that use enterprise login (SAML/SSO) now automatically fall back to arcgis.com authentication. No user action needed.

- **Missed Schedule Recovery** - Scheduled tasks now run automatically when the machine comes back online if a backup was missed due to sleep, shutdown, or reboot.

- **Overlap Protection** - When a scheduled backup fires while the previous run is still active, the new run proceeds with an overlap warning in the results. In rare cases where a process becomes stuck (no heartbeat for 30+ minutes), it is automatically terminated so it doesn't block future runs.

- **Non-English Windows Support** - Task Scheduler queries now use the COM API, fixing schedule display issues on non-English versions of Windows.

- **Scheduling and Credential Improvements** - Improved reliability for service account credentials, gMSA (experimental) and built-in account OAuth status display, multi-domain username handling, password retry without losing settings, and credential fill-in when editing schedules.

- **Schedule Drift Detection** - The application now detects when a Windows Task Scheduler entry has been modified externally (wrong trigger type, changed schedule time, disabled when it should be enabled). Drift is surfaced as a warning in the Schedules tab and flagged for repair in the startup dialog.

- **Stale Heartbeat Recovery** - If a previous scheduled backup process is stuck (alive but not making progress for 30+ minutes), it is now terminated automatically so the next run can proceed. Previously, a stuck process would block all future scheduled runs until the lock file was manually deleted.

- **Credential Key Preservation** - Deleting a schedule no longer removes the portal password from Windows Credential Manager if another schedule shares the same portal credentials. Per-schedule keys (OAuth tokens, S3/Azure keys, SMTP password) are still cleaned up normally.

- **Code Protection** - Core backup pipeline modules (export, retry, discovery, download, and orchestration - ~14,000 lines) are now compiled to native machine code, matching the protection already applied to licensing and authentication modules.

- **General Stability** - Numerous fixes across credential management, scheduling, GUI monitoring, and storage.

---

## Version 5.1.4

*Includes all features from [5.1.3](#version-513) and earlier.*

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f; font-size: 1.2em;">ACTION REQUIRED: Scheduled Backups Not Configured to Run When User is Logged Out</strong>

**Previous versions created Windows Task Scheduler tasks with "Run only when user is logged on". If no user was logged in to the machine at the scheduled time, the backup did not run.**

This version fixes the issue automatically. **When you open the application, it will detect all affected tasks and prompt you to fix them:**

1. A dialog lists every scheduled task that needs to be updated
2. Enter your **Windows username** (optional - leave blank to use the current user) and your **Windows login password**
3. Click **Update Tasks** - each task is deleted and recreated with "Run whether user is logged on or not"
4. A confirmation dialog shows how many tasks were updated and whether any failed
5. Your password is used only for task creation and is **not saved anywhere**

If you click **Skip**, you can fix tasks one at a time later:

- **In the application (recommended):** Go to the Schedules tab, click Edit on a schedule, re-enter the Windows username and password, and click OK
- **Directly in Task Scheduler:** You can also fix tasks manually. Open `taskschd.msc`, expand the **CivicLens** folder, right-click a task, select Properties, select "Run whether user is logged on or not", enter your password, and click OK. This works but the application will not know the task was fixed until the next restart, so the in-app method is preferred.

**To verify:** In Task Scheduler, right-click any task in the CivicLens folder, select Properties, and confirm it shows "Run whether user is logged on or not" under Security options.

If you enter the wrong password, the dialog will let you try again without losing your input. Tasks that were already updated successfully are not affected by a subsequent failed attempt.

</div>

- **Smarter Task Path Repair** - When the application is opened from a new location (after moving, renaming, or updating), the startup prompt now also updates the task path. Previously, path repair used a separate mechanism that could silently change the task's logon type.

- **Safer Task Updates** - When updating a scheduled task (e.g., fixing the logon type or path), the existing task is no longer deleted before creating the replacement. If the update fails (wrong password, account issue), the original working task is preserved instead of being lost.

- **Owner Filter Performance** - When an owner filter is set, the discovery phase now queries only that owner's items instead of scanning every user in the organization. Previously, setting an owner filter still retrieved the full user list and iterated all owners before filtering. This also applies to web map dependency scanning and group mapping, which are now scoped to the filtered owner and exit early when all items are mapped.

---

## Version 5.1.3

*Includes all features from [5.1.2](#version-512) and earlier.*

- **gMSA Support for Scheduled Tasks (Experimental)** - Group Managed Service Accounts (gMSA) are now supported for scheduled backups. Enter the account in `DOMAIN\account$` format in the Windows User field - no password is required. Active Directory manages the credentials automatically. This feature is experimental and may not work in all Active Directory configurations.

- **Cloud Credential Validation for Scheduled Backups** - S3 and Azure credentials are now checked when saving a schedule (access key format, connection string structure) with a warning if something looks wrong. At backup time, missing credentials (secret key or connection string) are detected at startup and logged clearly, rather than failing mid-backup with a generic message. If cloud storage is completely unreachable, the backup now falls back to local storage with a warning instead of aborting. Cloud credential info buttons now link directly to setup documentation.

- **Azure Download Reliability** - When restoring from Azure Blob Storage, partial files from interrupted downloads are now cleaned up automatically instead of being left on disk.

- **Windows Username Normalization** - The `.\` prefix (local account shorthand) is now automatically stripped from the Windows User field when creating scheduled tasks. This prevents a common configuration issue where `.\username` was rejected by Windows Task Scheduler while `username` worked correctly.

- **Faster Retry Processing** - The final retry pass now processes multiple items concurrently instead of sequentially. Previously, a small service could wait behind a multi-gigabyte export for nearly an hour. Items are now processed in parallel using a thread pool, matching the behavior of earlier retry passes.

- **Smarter Task Path Repair** - When the application is opened from a new location (after moving, renaming, or updating), scheduled tasks are now updated using the Windows Task Scheduler COM API instead of `schtasks /Change`. This fixes a silent failure where `schtasks` prompted for a password in the background and never completed. Disabled schedules are now repaired too, so they work correctly if re-enabled later. A notification dialog appears on startup showing how many tasks were updated and the current path, and the Schedules tab displays matching informational banners.

- **Safer Exit Handling** - Closing the application now distinguishes between manual and scheduled backups. Manual backups prompt for confirmation before cancelling. Scheduled backups show an informational message explaining they will continue in the background. When both are running, a combined dialog covers both scenarios.

- **Improved Cleanup Reliability** - Temporary portal items that were already deleted (by an earlier cleanup pass) no longer cause false "deletion failed" warnings. The cleanup now recognizes the ArcGIS "item does not exist" response as a successful outcome.

- **Improved Diagnostic Logging** - Full_Log.log now captures more detailed context for troubleshooting.

- **Startup Error Visibility** - If the application fails to start, the error log is now opened automatically in Notepad in addition to the error dialog. This ensures the error is visible even on systems where the dialog does not appear. The error dialog also uses topmost and foreground flags so it cannot be hidden behind other windows.

- **Broader OS Compatibility** - Fixed a startup failure (`ImportError: DLL load failed while importing QtWidgets`) on some Windows versions. The Qt UI framework has been pinned to a version with wider platform support.

---

## Version 5.1.2

*Includes all features from [5.1.0](#version-510) and [5.1.1](#version-511).*

- **Item Relationship Backup** - Backups now capture item relationships (e.g., Map2Service, Survey2Service, Solution2Item) alongside item data. For each item that has relationships, a `_relationships.json` file is saved to the Descriptions folder listing all forward and reverse relationships with their types, related item IDs, titles, and owners. After restoring an item that had relationships, a dialog shows the backed-up relationships and offers to recreate them on the target portal.

- **Splash Screen** - A splash screen now appears immediately when the application launches, providing visual feedback while PyInstaller extracts and initializes the application.

- **Resizable Table Columns** - All table column dividers across the application (Find & Replace results, backup progress, scheduled backup progress) are now draggable, allowing you to resize columns to fit your content.

- **EULA Dialog** - The EULA dialog now requires confirmation when closed via the window X button, matching the behavior of the Decline button.

- **Startup Error Diagnostics** - If the application fails to start (missing DLLs, GPU driver issues, import errors), a native Windows dialog now displays the full error with system diagnostics (OS version, architecture, VC++ runtime status, Qt plugin availability) and saves it to `%LOCALAPPDATA%\CivicLens\BackupUtility\StartErrorLog.txt`. The Qt platform plugin is validated before loading PySide6 to catch extraction failures early. Previously, the app would exit silently on startup failures.

- **Software OpenGL Fallback** - The application now automatically uses software OpenGL rendering on machines without GPU drivers (Windows Servers, VMs, RDP sessions). This prevents silent startup failures on headless environments.

- **Feature Service Symbology from Service Definition** - The "Symbology & Popups" restore option now restores `drawingInfo` from the service definition (`_def.json`) layer definitions, not just from item text (`_data.json`). The checkbox is now enabled whenever either file exists in the backup. Restore order: service settings first, layer-level symbology from definition second, item-level symbology from data last.

- **Portal Credential Fix** - Changing the password field in Restore or Find & Replace now correctly invalidates the cached portal connection. Previously, a stale connection could be used after changing the password.

- **Concurrent Backup Warning** - The overlap warning when starting a new backup now detects running scheduled backups in addition to on-demand backups.

- **Schedule Task Creation Diagnostics** - When Windows Task Scheduler fails to create a task (wrong password, account not found, access denied, service stopped, etc.), the error dialog now shows the specific cause and step-by-step troubleshooting instructions. Task creation is also verified after saving - if the task is missing despite a success report, a warning is shown immediately. Previously, raw Windows error text was shown without guidance.

- **Undo Diagnostic Messages** - After a restore, the console now explains whether undo is available and why (e.g., "Create backup before restoring" was not selected, or the safety backup did not complete).

---

## Version 5.1.1

### Improvements in 5.1.1

#### Scheduled Backup Monitoring

The scheduled backup progress tab now matches the on-demand backup tab exactly:

- **Full console output** - scheduled backups now display the same detailed console output as on-demand backups, including authentication messages, retry pass info, upload progress, and the full backup summary
- **Active Items** - downloading items, timeout countdowns, and all active item statuses now display correctly for scheduled backups
- **Skip breakdown** - skipped item counts are broken down into excluded, unsupported, and reference categories with a "Skip info" button, matching on-demand
- **Non-admin banner** - an informational banner appears when the backup runs with non-administrator credentials, noting that only items owned by the authenticated user will be backed up

#### Failure Notifications

- Failure emails and the StartErrorLog now display a structured report (title, date, organization, error) instead of a bare error string
- Fixed an issue where two failure emails were sent when a scheduled backup failed

---

## Version 5.1.0

### What's New in 5.1.0

#### Restore Tab

Restore backed-up items directly from the Backup Utility - no manual file editing or portal uploads required.

**Supported item types:**

- **JSON-based items** (Web Map, Dashboard, StoryMap, Web Experience, Web Mapping Application, QuickCapture Project, Hub Site/Page, GeoBIM, and more) - full item data, resources, and metadata restore
- **Feature Service** - configuration restore only (symbology, popups, service settings, domains, field properties). Feature data (rows and geometry) is never modified
- **Feature Layer View** - configuration restore, plus "Create as new" to recreate a view on a source Feature Service

**Two restore modes:**

- **Restore to existing item** - overwrites an existing portal item with backed-up configuration
- **Create as new item** - creates a new portal item from backup files, named `{title}_restored_{timestamp}`

**Safety features:**

- Automatic safety backup before every restore (saves current item state for rollback)
- If the safety backup fails, the restore is aborted and the item is not modified
- Create-as-new failures automatically clean up orphaned items
- Dependency checking warns about missing referenced items before creation

**Cloud restore:**

- Restore directly from Amazon S3 or Azure Blob Storage without downloading the full backup
- Only the small configuration files needed for restore are downloaded on demand
- Works with the same S3 and Azure credentials used for backup

**Cross-portal restore:**

- Create new items on a different ArcGIS portal or organization using the "Create on a different portal" option in create-as-new mode
- Both the source and target portals must have a Backup Utility license - short-term migration licenses are available at reduced cost for the target portal, contact [info@civiclens.com](mailto:info@civiclens.com) for details
- After creating items on the target portal, use the **Find & Replace** tab to update internal item ID references (web map layer sources, dashboard data sources, etc.) so they point to the correct items in the new organization

#### Find & Replace Tab

Scan your portal for references to old item IDs, service URLs, or other strings, and replace them across all dependent items.

- Read-only scan phase - nothing is modified until you explicitly apply
- Apply changes one item at a time with per-item safety backup and undo
- Auto-detects find/replace pair types (Item ID, URL, General) with validation
- Scope filters: item type, owner, specific item IDs
- Stale data check re-fetches items before applying to detect concurrent edits
- Session-based undo for every applied change

---

## Version 5.0

> See the [breaking change notice](#breaking-change-scheduled-backup-users-must-act-v50) at the top of this page if you use parameters file scheduling.

### What's New in 5.0

#### Redesigned Interface

New interface with three tabs:

- **Backup tab**: Configure connection, filters, storage, and start on-demand backups
- **Progress tab**: Real-time item-by-item status, active export tracking, and system metrics
- **Schedules tab**: Create, edit, and manage scheduled backups

#### Real-Time Progress Monitoring

The Progress tab shows live status for running backups:

- Progress bar with item counts (exported / failed / skipped / total)
- Active exports table with per-item API status (Pending, ExportingData, Completed, etc.)
- Phase indicator (Authenticating, Discovering, Exporting, Retry Pass 1-11, Finishing)
- System metrics (CPU, memory, disk)
- Console log with per-item results
- Elapsed time

Open the app during a scheduled backup to monitor its progress in real time.

#### Built-in Schedule Management

Schedules are now created and managed directly in the app.

- Daily, weekly, or monthly schedules
- **OAuth/SSO support** - scheduled backups can authenticate via your organization's SSO provider instead of storing a password
- Edit, delete, enable/disable, or Run Now from the GUI
- Overlap detection warns when a backup is still running at the next scheduled start
- Runs via Windows Task Scheduler under a `CivicLens` folder, even when logged out

#### Unified Application

Local, S3, Azure, cleanup, and scheduling consolidated into one executable.

#### Persistent Settings

All Backup tab configuration is saved automatically between sessions.

#### Exclude Item IDs

Exclude specific items by ArcGIS item ID (comma-separated).

### Improvements in 5.0

#### Migrating from Parameters Files

As noted in the [breaking change notice](#breaking-change-scheduled-backup-users-must-act-v50) above, parameters files are no longer auto-detected. Pass `--file` to continue using them, or migrate to the Schedules tab.

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

#### Cloud Storage

S3 and Azure Blob are now built in. S3 also supports IAM instance roles and environment credentials in addition to access keys.

#### Portable Schedules

Scheduled backups survive application moves and upgrades. On launch, the application automatically detects stale Windows Task Scheduler entries and updates them to point to the current executable path. Schedule configurations and credentials are stored in `%LOCALAPPDATA%` and Windows Credential Manager respectively, both independent of the install location.

#### Cleanup Safety

Retention validates paths before deletion, blocking drive roots, UNC roots, and protected directories (Desktop, Documents, etc.). Dry run previews what would be deleted.

#### Error Handling

- Authentication and license failures show actionable messages instead of tracebacks
- License validation uses S3 fallback and local cache for resilience

#### Reliability

- Memory-aware downloads with adaptive chunk sizes and garbage collection
- CPU pressure monitoring (memory only in v4.x)
- Disk full detection stops cleanly instead of writing corrupt files

#### Security

All credentials stored in **Windows Credential Manager**, encrypted via **Windows DPAPI**. Never written to config files, logs, or the registry.

- **Per-schedule isolation**: Unique keyring entry per schedule
- **OAuth token rotation**: New refresh tokens automatically persisted, stale tokens replaced
- **Code-signed**: CivicLens LLC EV certificate with SHA-256 timestamping
- **No telemetry**: Communicates only with your ArcGIS environment. Exceptions: license validation (checks locally, org name not transmitted), version check, and opt-in CivicLens email notifications

---

## Version 4.x

Version 4.x was the previous generation of the Backup Utility, distributed as separate executables per storage backend and scheduling mode.

### Separate Executables

Each combination of storage and scheduling mode was a separate application:

- `od.exe` - on-demand backup to local storage
- `od_s3.exe` - on-demand backup to Amazon S3
- `od_azure.exe` - on-demand backup to Azure Blob Storage
- `sch.exe` - scheduled backup to local storage (headless, via parameters file)
- `sch_s3.exe` - scheduled backup to Amazon S3
- `sch_azure.exe` - scheduled backup to Azure Blob Storage
- `od_cleanup.exe` / `sch_cleanup.exe` - retention cleanup

All of these are consolidated into a single `BackupUtility.exe` in v5.0.

### Scheduling via Parameters File

Scheduled backups were configured through an Excel or CSV parameters file and executed via Windows Task Scheduler. The user was responsible for creating the Task Scheduler entry manually.

### Key Capabilities (carried forward to v5.0)

- Full ArcGIS Online and Portal for ArcGIS content backup
- Feature Service export via File Geodatabase (FGDB) with multi-format fallback (CSV, Shapefile)
- 11-pass retry logic for transient failures, permission issues, and corrupt features
- Web Map, Dashboard, StoryMap, Web Experience JSON export
- Survey123 form export
- Notebook export
- Static item download with resume support
- Email notifications with backup summary
- Retention-based cleanup of old backups
- Inventory CSV with per-item export details

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
