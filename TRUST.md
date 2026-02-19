# Trust & Security

This page explains what the application does, what it doesn't do, and how you can verify each claim.

---

## What the Application Does

Backup Utility connects to your ArcGIS Online or Portal for ArcGIS environment, exports your content (maps, apps, feature services, files), and saves it to a location you choose: a local folder, Amazon S3, or Azure Blob Storage.

## What It Does Not Do

- **No telemetry.** The application does not collect usage data, analytics, or diagnostics.
- **No data transmission.** Your ArcGIS content and credentials are never sent to CivicLens or any third party. Backups go only where you tell them to.
- **No background services.** Nothing is installed. No Windows services, no registry entries, no startup items. Scheduled backups run through Windows Task Scheduler, which you can inspect directly.
- **No installer.** The application is a single `.exe` file. Delete it and it's gone.

## Network Connections

The application makes exactly three types of outbound connections:

| Destination | Purpose | Data sent | When |
|---|---|---|---|
| **Your ArcGIS portal** | Export your content | Your credentials (to your own portal) | During backup/restore |
| **ArcGIS Online hosted service** | License validation (primary) | Nothing - downloads a license list, checks your org name locally | On startup |
| **CivicLens S3 bucket** | License validation (fallback) | Nothing - downloads a cached JSON copy of the same list | If primary check is unavailable |
| **CivicLens CDN** | Update check | Nothing - checks a version number | On startup |

If you enable email notifications through CivicLens mail, result summaries are sent through our mail server. You can avoid this entirely by using the **Custom SMTP** option with your own mail server.

**Optional cloud storage connections** (S3 or Azure) go directly to your own storage accounts using credentials you provide. CivicLens never sees or proxies this traffic.

## Credential Security

All credentials are stored in **Windows Credential Manager**, encrypted at rest using **Windows DPAPI** (Data Protection API). This is the same mechanism used by Windows itself, Chrome, Edge, and other applications for credential storage.

- Passwords are never written to config files, log files, or the Windows registry.
- Each scheduled backup uses a unique credential entry that you can inspect in Credential Manager.
- Proxy credentials are redacted from log output.
- ArcGIS tokens are redacted from error tracebacks in log files.

### How to verify

1. Open **Control Panel > Credential Manager > Windows Credentials**
2. Look for entries starting with `BackupUtility_` - these are the only credentials the application stores
3. You can delete any of them at any time

---

## Code Signing

The executable is signed with a **CivicLens LLC EV code signing certificate** issued by a trusted Certificate Authority (Sectigo). The digital signature guarantees the executable has not been tampered with or modified since it was built. You can verify by right-clicking `BackupUtility.exe` > **Properties** > **Digital Signatures** - the signer should be **CIVICLENS LLC**.

---

## No Hidden Processes

The application runs as a single process. Scheduled backups run as separate processes through Windows Task Scheduler, which you fully control.

### How to verify

1. Open **Task Manager** while the application is running - you'll see one `BackupUtility.exe` process (plus one per running scheduled backup)
2. Open **Task Scheduler > Task Scheduler Library > CivicLens** to see all scheduled tasks - each shows the exact command that runs
3. There are no Windows services, no background agents, and nothing in your Startup folder

---

## File System Footprint

The application touches only these locations:

| Location | What | Removable? |
|---|---|---|
| Wherever you saved `BackupUtility.exe` | The application itself | Delete the file |
| Your chosen backup folder | Your backup data | Delete the folder |
| `%LOCALAPPDATA%\CivicLens\BackupUtility\` | Schedule configs, status files, diagnostics | Delete the folder |
| Windows Credential Manager | Stored passwords (entries prefixed `BackupUtility_`) | Delete individually |
| Windows Task Scheduler `\CivicLens` folder | Scheduled tasks | Delete through Task Scheduler |

To completely remove the application: delete the exe, delete the `%LOCALAPPDATA%\CivicLens` folder, remove Credential Manager entries starting with `BackupUtility_`, and delete the `CivicLens` folder in Task Scheduler.

---

<p style="text-align: center; color: #888; margin-top: 40px;">
Backup Utility for ArcGIS | CivicLens LLC | v5.1.3
</p>
