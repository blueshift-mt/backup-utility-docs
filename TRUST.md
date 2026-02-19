# Trust & Security

You're being asked to give this application your ArcGIS credentials and access to your organization's data. That's a significant ask. This page explains exactly what the application does, what it doesn't do, and how you can verify every claim yourself.

---

## What the Application Does

Backup Utility connects to your ArcGIS Online or Portal for ArcGIS environment, exports your content (maps, apps, feature services, files), and saves it to a location you choose: a local folder, Amazon S3, or Azure Blob Storage. That's it.

## What It Does Not Do

- **No telemetry.** The application does not collect usage data, analytics, or diagnostics.
- **No data transmission.** Your ArcGIS content is never sent to CivicLens or any third party. Backups go only where you tell them to.
- **No background services.** Nothing is installed. No Windows services, no registry entries, no startup items. Scheduled backups run through Windows Task Scheduler, which you can inspect directly.
- **No installer.** The application is a single `.exe` file. Delete it and it's gone.

## Network Connections

The application makes exactly three types of outbound connections:

| Destination | Purpose | Data sent | When |
|---|---|---|---|
| **Your ArcGIS portal** | Export your content | Your credentials (to your own portal) | During backup/restore |
| **ArcGIS Online hosted service** | License validation | Nothing - downloads a list, checks locally | On startup |
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

The executable is signed with a **CivicLens LLC Extended Validation (EV) code signing certificate**. EV certificates require verified business identity - the certificate authority confirms the legal entity, physical address, and authorized signers before issuing.

This means:
- Windows SmartScreen will not block the application
- The publisher name "CIVICLENS LLC" appears in Windows UAC prompts
- The signature proves the executable has not been modified since it was built

### How to verify

1. Right-click `BackupUtility.exe` > **Properties** > **Digital Signatures** tab
2. You should see:
   - **Name of signer:** CIVICLENS LLC
   - **Timestamp:** A date/time from Sectigo's timestamp server
3. Click **Details** > **View Certificate** to inspect the full certificate chain
4. Or from a command prompt:
   ```
   signtool verify /pa /v BackupUtility.exe
   ```

If the signature is missing or shows a different signer, do not run the executable.

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

## About CivicLens

CivicLens LLC is a Montana-based company focused on GIS data management tools for state and local government.

- **Website:** [civiclens.com](https://civiclens.com)
- **Contact:** info@civiclens.com

---

<p style="text-align: center; color: #888; margin-top: 40px;">
Backup Utility for ArcGIS | CivicLens LLC | v5.1.3
</p>
