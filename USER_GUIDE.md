# Backup Utility for ArcGIS Online and Portal for ArcGIS

## User Guide | Version 5.1.5

---

## Quick Start (On-Demand Backup)

See [Scheduling Automatic Backups](#scheduling-automatic-backups) for scheduled backups.

### Step 1: Download & Launch

1. Download **BackupUtility.exe** from [civiclens.com/downloads](https://civiclens.com/downloads)
2. Save it anywhere on your computer
3. Double-click to launch

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Note for parameters file users:</strong> The application no longer auto-detects parameters files. To run a headless backup with a parameters file, use <code>BackupUtility.exe --file parameters.xlsx</code>. Parameters file support is temporary and will be removed in a future release - please migrate to the built-in Schedules tab.

</div>

### Step 2: Connect to Your Organization

| Field | What to Enter |
|-------|---------------|
| **Portal URL/Organization** | **ArcGIS Online:** Your org name (e.g., `myorg`) or full URL (e.g., `myorg.maps.arcgis.com`). **Portal:** Your Portal URL (e.g., `https://gis.mycompany.com/portal`) |
| **Username** | Your ArcGIS username (case-sensitive) |
| **Password** | Your password, or leave blank and check SSO/OAuth for single sign-on |

### Step 3: Choose Where to Save

Click **Browse** to select a backup folder. Local paths (e.g., `D:\Backups`) and UNC paths (e.g., `\\server\share\backups`) both work.

> **Warning**: Don't select a drive root like `C:\` or network share roots like `\\server\share`.

### Step 4: Start Backup

Click **Start Backup**. The Progress tab shows real-time status. Results.txt and Inventory.csv are generated on completion.

---

## System Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended)
- Network access to ArcGIS Online or Portal

---

## What Gets Backed Up

> Type names shown in `backticks` are the exact strings used by the **Exclude Types** filter.

### Hosted Feature and Table Data
Exported as **File Geodatabase (.gdb)** with domains, attachments, and related tables:
`Feature Service`

### Location Tracking Services
Exported as **Shapefile (.shp)** to a `Location Tracking` folder. The ArcGIS export API does not support File Geodatabase for Location Tracking Services.

### Tile & Map Services
Exported as **Tile Package (.tpk or .vtpk)**:
`Map Service`, `Vector Tile Service`

### Web Apps, Maps & Dashboards
Exported as **JSON configuration with item resources**:
- **Maps**: `Web Map`, `Web Scene` (Field Maps configurations are included in web map item resources)
- **Apps**: `Dashboard`, `StoryMap`, `Web Experience`, `Web Mapping Application`, `Mobile Application`
- **Field Apps**: `QuickCapture Project`, `Workforce Project`
- **Insights**: `Insights Workbook`, `Insights Page`, `Insights Model`, `Insights Theme`
- **Hub/Sites**: `Hub Site Application`, `Hub Page`, `Hub Initiative`, `Hub Initiative Template`, `Hub Event`, `Site Application`, `Site Page`
- **Other**: `Solution`, `Mission`, `Investigation`, `GeoBIM Project`, `GeoBIM Application`, `Data Pipeline`, `Style`, `StoryMap Theme`, `Map Area`, `Symbol Set`, `Color Set`, `Content Category Set`, `Web Experience Template`, `Group Layer`, `Feature Collection`, `Feature Collection Template`, `360 VR Experience`, `Oriented Imagery Catalog`

### Survey123 Forms
Downloaded as **survey packages** including XLSForm, media, pulldata files, CSVs, and JSON configuration:
`Form`

### Documents
Downloaded in **original format**:
- **Office**: `Microsoft Word`, `Microsoft Excel`, `Microsoft Powerpoint`, `Visio Document`, `PDF`
- **Apple**: `iWork Keynote`, `iWork Pages`, `iWork Numbers`
- **Images**: `Image`, `Image Collection`
- **Other**: `CAD Drawing`, `Report Template`, `Administrative Report`

### GIS Data Files
Downloaded in **original format**:
`Shapefile`, `File Geodatabase`, `GeoPackage`, `GeoJson`, `KML`, `KML Collection`, `GML`, `Apache Parquet`, `SQLite Geodatabase`

### Tabular Data
Downloaded in **original format**:
`CSV`, `CSV Collection`

### ArcGIS Packages
Downloaded in **original format**:
- **Project**: `Project Package`, `Project Template`, `Pro Map`, `ArcGIS Pro Configuration`
- **Map/Layer**: `Map Package`, `Map Template`, `Layer`, `Layer Package`, `Map Document`
- **Scene**: `Scene Package`, `Mobile Scene Package`
- **Mobile**: `Mobile Map Package`, `Mobile Basemap Package`
- **Tile**: `Tile Package`, `Vector Tile Package`, `Compact Tile Package`
- **Analysis**: `Locator Package`, `Geoprocessing Package`, `Geoprocessing Package (Pro version)`, `Geoprocessing Sample`
- **Other**: `Rule Package`, `Deep Learning Package`, `Export Package`, `Task File`, `Layout`, `Pro Report`, `Desktop Style`, `Raster function template`, `Workflow Manager Package`, `Statistical Data Collection`, `Insights Workbook Package`
- **Legacy**: `ArcPad Package`, `Explorer Map`, `Explorer Layer`, `Globe Document`

### Service Definitions
Downloaded in **original format**:
`Service Definition`

### Add-ins
Downloaded in **original format**:
`Desktop Add In`, `Explorer Add In`, `ArcGIS Pro Add In`, `Survey123 Add In`, `AppBuilder Widget Package`, `Experience Builder Widget`, `Experience Builder Widget Package`

### Notebooks
Downloaded as **.ipynb files**:
`Notebook`, `Notebook Code Snippet Library`

Code items (`Code Sample`, `Code Attachment`) may not have an associated file. If none exists, only item metadata is saved.

### Other
Downloaded in **original format**:
`Urban Project`, `CityEngine Web Scene`, `Native Application`, `Native Application Installer`, `Desktop Application`, `Desktop Application Template`

### Not Exportable

**Reference-only** (item description saved, no data):
`WMS`, `WMTS`, `WFS`, `Geocoding Service`, `Document Link`, `Map Image Layer`

**Not exportable** from the hosted ArcGIS environment:
`Scene Service`, `Scene Layer`, `Image Service`

**Not included** in backups:
`Application`, `API Key` - developer credentials (OAuth/API registrations), not web apps.

**Conditionally skipped** during export:
- Referenced Feature Services (data lives in another org or external server)
- Multipatch Feature Services (3D geometry not supported by the export API)
- Tile layers exceeding the ArcGIS Online export limit
- Unpublished apps

Each backup includes an **Inventory.csv** listing every item with its ID, title, owner, type, dates, and backup result.

---

## Logging In

### Regular Password Login

Enter your username and password. **Username is case-sensitive.**

### SSO / Multi-Factor Authentication

For organizations using single sign-on or MFA-enforced login:

1. Leave the password field **empty**
2. Check the **SSO / OAuth** checkbox
3. Click **Start Backup** - a browser window opens
4. Complete your organization's login
5. The app receives your credentials and backup starts

### OAuth for Scheduled Backups

Scheduled backups run unattended. OAuth uses a stored refresh token:

1. In the **Schedules** tab, check **SSO / OAuth**
2. Click **Authorize...**
3. Complete your organization's login in the browser
4. The utility stores a refresh token securely in Windows Credential Manager
5. Scheduled backups will automatically refresh the token as needed

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">Refresh tokens expire after approximately 2 weeks of inactivity.</strong> If no backup runs within that window, the schedule will fail until you re-authorize. Credential changes and SSO provider changes also invalidate tokens.

</div>

### Windows Credential Manager (Recommended for Automation)

Store passwords securely instead of typing them:

1. Open **Windows Credential Manager** (Control Panel > Credential Manager)
2. Click **Add a generic credential**
3. Enter:
   - **Internet or network address**: `CIVICLENS`
   - **User name**: Your ArcGIS username
   - **Password**: Your password
4. In the Backup Utility, enter `wcm` as your password

<div style="border: 2px solid #2e7d32; border-left: 6px solid #2e7d32; background-color: #e8f5e9; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #2e7d32;">Security Tip:</strong> Recommended for scheduled backups. Credentials are encrypted at rest using Windows DPAPI, tied to your Windows user account, and never leave your machine.

</div>

### Admin vs. Non-Admin Mode

Detected automatically:

- **Admin**: Backs up all organization content
- **Non-Admin**: Backs up only items you own

Non-admin accounts see a message like:

```
username is not an administrator for orgname. Running in non-admin mode.
```

The backup proceeds with accessible content.

---

## Filtering What to Back Up

### Full vs. Incremental Backup

- **Full Backup**: Everything in your organization
- **Incremental Backup**: Only items modified within a specified number of days

Set **Incremental Days** to back up only recent changes (e.g., `7` for the last week).

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Note:</strong> Incremental backup is only available for ArcGIS Online. A 12-hour buffer is added automatically to avoid missing items near the boundary.

</div>

### Filter by Item Type

Choose from:
- **All Items**
- **All Items Except Cached Tile Layers**
- **Feature Services Only**

### Scan Type

- **Quick**: Standard ArcGIS search. Faster, works well for most organizations
- **Deep**: Paginates through all content folders. Required for organizations with 10,000+ items

> **Tip**: If your backup seems to miss items, try Deep Scan.

### Filter by Tag

**Include tags**: Only backup items with these tags
```
production, approved
```

**Exclude tags**: Skip items with these tags
```
test, draft
```

### Filter by Owner, Group, or Folder

- **Owner**: Enter a username to back up only that person's content
- **Groups**: Enter group names separated by semicolons
- **Folders**: Enter folder names separated by semicolons

```
Public Works;Planning;GIS Team
```

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Note:</strong> Groups and Folders filters are mutually exclusive - use one or the other.

</div>

### Exclude Groups or Specific Items

- **Exclude Groups**: Skip items shared to specific groups (semicolon-separated)
- **Exclude Item IDs**: Skip specific items by their ArcGIS item ID (comma-separated)

```
# Exclude groups containing test data
Test Group;Development;Staging

# Exclude specific item IDs
a1b2c3d4e5f6,b2c3d4e5f6a7
```

### Delete Protection

Control delete protection on items during backup:

- **No changes**: Leave protection settings as-is (default)
- **Enable**: Turn on delete protection for all items in the backup
- **Disable**: Remove delete protection from all items

### Exclude Types

Skip specific item types from backup. Enter type names separated by commas:

```
Web Map, Dashboard, StoryMap
```

Useful for backing up feature data only.

### Advanced Options

Found in the **Advanced** section of the Backup tab:

| Option | Description |
|--------|-------------|
| **Service Definitions** | Download .sd files from ArcGIS Pro |
| **Write Dependencies** | Record item-to-item dependencies in Inventory.csv |
| **Empty Services** | Include feature services with zero features (preserves schema). Enabled by default |
| **Tag Backed-Up Items** | Tag items with `last_backup_<date>` after export. ⚠️ Updates modified date, so every tagged item appears in the next incremental backup |

---

## Scheduling Automatic Backups

### Create a Schedule

1. Go to the **Schedules** tab
2. Click **Add Schedule**
3. Enter a schedule name, your ArcGIS organization URL, and username
4. Set the frequency (Daily, Weekly, or Monthly) and time
5. Choose a save path and storage type
6. Enter your **Windows login password** when prompted
7. Click **Save**

Schedules run via Windows Task Scheduler, even when you are logged out or the screen is locked.

#### Windows Credentials

Each schedule requires your **Windows login password** so Task Scheduler can run the backup unattended. This is your Windows password, not your ArcGIS password. Optionally specify a **Windows User** to run under a different account (defaults to current user).

If you enter the wrong password, a dialog lets you try again without losing any of your schedule configuration. If you skip the password entirely, the schedule is saved but backups will only run while you are logged in.

> **Tip**: Your password is used only to create the Windows scheduled task and is never stored by the Backup Utility. Windows Task Scheduler stores the credentials securely.

#### Service Accounts

Service accounts are fully supported. When you specify a different Windows user in the schedule dialog, the application stores ArcGIS and cloud storage credentials in that account's Windows Credential Manager so the scheduled task can access them at runtime.

**Requirements for the service account:**
- Must have the **Log on as a batch job** right (Local Security Policy > User Rights Assignment)
- Must have logged on to the machine at least once (so its user profile exists)
- Must have read access to the backup save path

**gMSA (Group Managed Service Accounts)** support is experimental. Enter the account name as `DOMAIN\account$` - no Windows password is required. Credentials are stored using machine-scope encryption so the gMSA can access them at runtime. This feature may not work in all Active Directory configurations - please contact support@civiclens.com if you encounter issues.

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Monitor Running Backups:</strong> Open the application at any time to check progress on the <strong>Progress</strong> tab. Closing the GUI does not stop the backup.

</div>

> **Tip**: Set backups to run overnight when network usage is low.

> **Overlap Protection**: If a backup is still running when the next scheduled run starts, the new run is automatically skipped. The skip is recorded in the schedule's status with the reason visible in the Schedules tab.

<a id="upgrading-and-moving-the-application"></a>

### Upgrading & Moving the Application

Schedule configurations, credentials, and settings are stored independently of the application:

| Component | Location | Survives upgrade? |
|-----------|----------|-------------------|
| Schedule configs | `%LOCALAPPDATA%\CivicLens\BackupUtility\schedules.json` | Yes |
| ArcGIS credentials | Windows Credential Manager (per-user) | Yes |
| Service account credentials | Replicated to the service account's Credential Manager | Yes |
| Windows Task Scheduler tasks | Task Scheduler `\CivicLens` folder | Repaired on startup |

Scheduled tasks run from whichever copy of BackupUtility was last opened. If you move or rename the application or its folder, open it from the new location so your scheduled tasks are updated. If you download an update to the same location (replacing the existing file), no action is needed.

**How it works:** Each time the application starts, it checks whether all scheduled tasks (enabled and disabled) are healthy. It detects three issues:

- **Stale path** - the task points to a different application location (after moving or updating)
- **Missing task** - the Windows Task Scheduler task does not exist
- **Runs only when logged on** - the task is configured to run only when a user is logged in, meaning it will not run at the scheduled time if no one is logged in

If any issues are found, a dialog appears when the application starts:

1. The dialog lists every scheduled task that needs to be fixed
2. Enter your **Windows login password** (and optionally a different Windows username)
3. Click **Update Tasks** - each task is recreated with the correct settings
4. A confirmation shows how many tasks were updated and whether any failed

If you enter the wrong password, the dialog lets you try again. If you click **Skip**, you can fix tasks later by editing each schedule in the Schedules tab and re-entering the password. Your password is used only for task creation and is not saved.

**Ongoing monitoring:** The Schedules tab Warnings column checks task health every time it refreshes. If a task is missing, runs only when logged on, or points to the wrong location, a warning appears in the table. This catches issues that occur between application restarts (for example, if someone modifies a task directly in Task Scheduler).

**If a scheduled backup fires before you open the app from the new location**, that run will fail because the old path no longer exists. The next time you open the application, the task is repaired, and all future runs will use the correct path.

The application can be stored on a local drive or a network share. Network shares work because scheduled tasks run under the Windows user account specified during schedule creation. Keep in mind that if the network is unavailable when a scheduled backup fires, the backup will not run.

### Where to Find Scheduled Tasks

Tasks are created in Windows Task Scheduler under the **CivicLens** folder:

1. Open **Task Scheduler** (search in Start menu)
2. Expand **Task Scheduler Library** > **CivicLens**
3. Tasks are named `BackupUtility_<id>`

### Email Confirmation

Receive email when backups complete or fail.

### Managing Schedules

- **Edit**: Modify settings. If a backup is currently running, you are warned that changes apply to future runs only
- **Delete**: Removes the schedule and its Windows Task Scheduler task. If the task cannot be removed, you are told how to delete it manually
- **Run Now**: Run the backup immediately through the GUI with progress monitoring
- **Enable/Disable**: Pause without deleting. Enabling a schedule that has no Windows task prompts for your Windows password to create one

### Troubleshooting Task Scheduler Errors

If task creation fails, the application saves your schedule configuration and shows a warning with the specific error and troubleshooting steps. If the error is a wrong password, you can re-enter it immediately without losing your settings. For other errors, you can generate an XML file to import the task manually.

**Common errors and solutions:**

| Error | Cause | Solution |
|-------|-------|----------|
| **User name or password is incorrect** | Wrong password, nonexistent account, or Windows Hello blocking password auth. Windows uses the same error for all three cases. | See [Troubleshooting user name or password is incorrect](#troubleshooting-user-name-or-password-is-incorrect) below |
| **Account not found** | Username cannot be resolved (typo, deleted account, or missing domain prefix) | Check spelling, include the domain or machine name (e.g., `DOMAIN\user` or `.\localuser`) |
| **Account locked out** | Too many failed login attempts | Ask your IT administrator to unlock the account, or wait for the lockout period to expire |
| **Password expired** | The account password must be changed | Log in with the account to set a new password, then update the schedule |
| **Logon session does not exist** | The account lacks the "Log on as a batch job" right | Open **Local Security Policy** (secpol.msc) > Local Policies > User Rights Assignment, and add the account to **Log on as a batch job** |
| **Credential warning for service account** | Could not store credentials in the service account's Credential Manager | Ensure the service account has logged on to this machine at least once to create its user profile |
| **Access denied** | Group Policy restricts task creation, or account permissions are insufficient | Check with your IT administrator about Group Policy restrictions. Running as administrator may help. |
| **Task Scheduler service not running** | The Windows service is stopped or disabled | Open **services.msc**, find **Task Scheduler**, right-click and select **Start**. Set Startup Type to **Automatic** |

**Manual import option:** When automatic task creation fails, click **Yes** on the warning dialog to generate an XML file. You (or an administrator) can import it via Task Scheduler (right-click Task Scheduler Library > Import Task) or from an elevated command prompt:

```
schtasks /Create /TN "CivicLens\BackupUtility_<id>" /XML "path\to\file.xml"
```

### Troubleshooting user name or password is incorrect

This is the most common task creation error. Windows uses the same error message for several different problems, which makes it frustrating to diagnose. Work through the steps below in order, or skip straight to the **quick alternative** at the end.

#### Step 1: Confirm you're entering the right password

Task Scheduler needs the password you use to **sign in to this machine**, which is not always the password you expect:

- **Not your Windows Hello PIN** - Task Scheduler requires a traditional password, not a PIN, fingerprint, or face scan
- **Not necessarily your Microsoft account password** - If the machine uses a local Windows account, your Microsoft online password is a completely separate credential
- **Not your ArcGIS password** - This is your Windows login password, not your portal password

**Quick test:** Lock your computer (Win+L), click **Sign-in options** on the lock screen, click the key icon to switch to password entry, and try the password there. If it works at the lock screen, it will work in BackupUtility. If it doesn't, the password is wrong and Task Scheduler will reject it too.

> **Not sure which account type you have?** Open **Settings > Accounts > Your info**. It will show "Microsoft account", "Local account", or your organization name. This determines which password to use:

| Machine setup | Password to use |
|---------------|-----------------|
| **Microsoft account** | Your Microsoft account password (the one you use at outlook.com, microsoft.com, etc.) |
| **Local Windows account** | The password set locally on this machine. Your Microsoft account password has no effect here. |
| **Azure AD / Entra ID** (work or school) | Your organization password |
| **Active Directory domain** | Your domain password |

#### Step 2: Check if Windows Hello is blocking password authentication

On Windows 10 and 11, a setting called **"Only allow Windows Hello sign-in for Microsoft accounts on this device"** can block password authentication entirely. When this is enabled, Task Scheduler will reject every password you enter, even if it's correct.

**How to check:** Open **Settings > Accounts > Sign-in options** and look for this toggle.

If it's **on**, you have three options:

**A. Disable the setting and use your password:**

1. Turn **off** "Only allow Windows Hello sign-in"
2. **Sign out or restart** your computer (required - the password option won't appear at the lock screen until you do)
3. Log in with your password to confirm it works
4. Open BackupUtility and save your schedule
5. You can re-enable the Hello-only setting afterward - it only affects task *creation*, not execution of existing tasks

> If the toggle is greyed out, your organization enforces it via Group Policy or Intune. Contact your IT department, or use option B or C below.

**B. Enter SYSTEM in the Windows User field (requires running as administrator):**

This bypasses the password requirement entirely. See [Using SYSTEM](#using-system-as-the-run-as-account) below.

**C. Use a dedicated service account:**

Domain service accounts and Group Managed Service Accounts are typically not affected by Windows Hello policies. Enter the service account's username and password in the schedule dialog.

#### Step 3: If you don't know your Windows password

If you've been using a PIN or biometric for a long time, you may no longer remember your traditional password, or it may have expired without you noticing. How to reset it depends on your account type:

| Account type | How to reset |
|-------------|-------------|
| **Microsoft account** | At the Windows lock screen, click **"I forgot my password"** and follow the prompts. Windows sends a verification code to your recovery email or phone. You must be connected to the internet. |
| **Local Windows account** | At the lock screen, click **"I forgot my password"** and answer your security questions. If you don't remember them, an administrator on the machine must reset it. |
| **Azure AD / Entra ID** | Contact your IT department for a password reset. |
| **Active Directory domain** | Contact your IT department, or press Ctrl+Alt+Del > Change Password if you know the current password. |

<div style="background-color: #1565c0; color: white; padding: 12px 16px; border-radius: 6px; margin: 12px 0;">
<b>Microsoft account vs. local account:</b> Resetting your password at <a href="https://account.microsoft.com" style="color: #bbdefb;">account.microsoft.com</a> only works if the machine is signed in with a Microsoft account. If it uses a local Windows account, the online reset has <b>no effect</b> on the machine password. Check <b>Settings > Accounts > Your info</b> to see which type you have.<br><br>
<b>Online reset not syncing?</b> If you reset your Microsoft account password online but the new password isn't accepted at the login screen, make sure the machine is connected to the internet and restart. Windows must contact Microsoft's servers to sync the new password.
</div>

#### Step 4: Other causes

If none of the above resolved the issue:

- **Account locked out** - Too many failed attempts can lock the account. Wait for the lockout period to expire or ask your IT admin to unlock it.
- **Password expired** - The account password may have expired. Log in with the account to set a new one (Ctrl+Alt+Del > Change Password for domain accounts).
- **Username doesn't exist** - Windows returns the same "incorrect" error for nonexistent usernames. Verify spelling and include the domain if needed (`DOMAIN\username`).
- **Contact support** - Email support@civiclens.com with the exact error message shown in the dialog.

#### Using SYSTEM as the run-as account

If password authentication is problematic for any reason, you can skip it entirely by running the scheduled task as the Local System account.

> **Requires administrator:** Creating a task that runs as SYSTEM requires BackupUtility to be running as administrator. Right-click BackupUtility and select **Run as administrator** before saving the schedule.

1. In the schedule dialog, enter `SYSTEM` in the **Windows User** field
2. Leave the **Windows Password** field blank
3. Click **Save**

SYSTEM runs whether or not any user is logged in, and has access to all local files and folders. Your ArcGIS portal credentials and any cloud storage credentials (S3, Azure) are stored securely using machine-scope encryption so the scheduled backup can still authenticate to ArcGIS Online, Portal, S3, and Azure exactly as it would under your own account.

> **Network shares:** Mapped drives (e.g., `Z:\backups`) are per-user and not available to SYSTEM. UNC paths (e.g., `\\server\share`) can work if the share grants access to the computer account (`COMPUTERNAME$`). If your save path is a network share with user-specific permissions, use your own account or a service account instead. Saving to a local folder, S3, or Azure is unaffected.

---

## Cloud Storage

### Save Backups to Amazon S3

#### Step 1: Create an S3 Bucket (if needed)

1. Log in to the [AWS Console](https://console.aws.amazon.com)
2. Go to **S3** and click **Create bucket**
3. Enter a unique bucket name (e.g., `mycompany-arcgis-backups`)
4. Choose your region
5. Keep default settings and click **Create bucket**

#### Step 2: Create an IAM Policy (Recommended)

For better security, create a policy that only allows access to your backup bucket:

1. Go to **IAM** > **Policies** > **Create Policy**
2. Click the **JSON** tab and paste:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:GetObjectAcl",
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR-BUCKET-NAME",
                "arn:aws:s3:::YOUR-BUCKET-NAME/*"
            ]
        }
    ]
}
```

3. Replace `YOUR-BUCKET-NAME` with your actual bucket name
4. Click **Next**, give it a name (e.g., `BackupUtilityS3Access`), and **Create policy**

#### Step 3: Create an IAM User for Backups

1. Go to **IAM** > **Users** > **Create user**
2. Enter a name (e.g., `backup-utility`)
3. Select **Attach policies directly**
4. Search for and select the policy you just created (or use `AmazonS3FullAccess` for simplicity)
5. Click **Create user**
6. Click on the user, go to **Security credentials**
7. Click **Create access key** > **Application running outside AWS**
8. Copy the **Access Key ID** and **Secret Access Key** (save these securely - the secret is only shown once)

#### Step 4: Configure Backup Utility

1. In the Backup Utility, select **Amazon S3** as storage type
2. Enter:
   - **Bucket name**: Your bucket name (e.g., `mycompany-arcgis-backups`)
   - **Subdirectory** (optional): A path within the bucket (e.g., `prod/weekly`)
   - **Access Key ID**: From the IAM user
   - **Secret Access Key**: From the IAM user

<div style="border: 2px solid #2e7d32; border-left: 6px solid #2e7d32; background-color: #e8f5e9; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #2e7d32;">Tip:</strong> Leave credentials blank if running on EC2/ECS with an IAM role attached - the utility will use instance credentials automatically.

</div>

#### Optional: Store S3 Credentials in Windows Credential Manager

Instead of entering credentials in the app, store them securely:

1. Open **Windows Credential Manager** (Control Panel > Credential Manager)
2. Click **Add a generic credential**
3. Enter:
   - **Internet or network address**: `CIVICLENS_S3`
   - **User name**: Your S3 Access Key ID
   - **Password**: Your S3 Secret Access Key
4. In the Backup Utility, enter `wcm` as the Secret Access Key

#### Optional: Set Up S3 Lifecycle Rules for Cost Savings

Move old backups to cheaper storage automatically:

1. In your S3 bucket, go to **Management** > **Create lifecycle rule**
2. Name the rule and select **This rule applies to all objects in the bucket**
3. Check **Transition current versions of objects between storage classes**
4. Choose a storage class:
   - **S3 Standard-IA**: For backups accessed less than once a month
   - **S3 Glacier Instant Retrieval**: For rare access with instant retrieval
   - **S3 Glacier Deep Archive**: Cheapest option, 12-hour retrieval time
5. Set the number of days after creation to transition (e.g., 30 or 90 days)
6. Click **Create rule**

> **Note**: Glacier storage classes have per-object retrieval fees. Use for long-term archival only.

#### Recommended: Browse S3 Backups with WinSCP

[WinSCP](https://winscp.net) is a free Windows file manager that connects directly to S3. It provides a familiar Explorer-like interface for browsing, downloading, and managing your backup files without using the AWS Console or CLI.

1. Download and install [WinSCP](https://winscp.net/eng/download.php)
2. Open WinSCP and select **Amazon S3** as the protocol
3. Enter your **Access Key ID** and **Secret Access Key** (the same credentials used in the Backup Utility)
4. Click **Login** to connect
5. Navigate to your backup bucket and browse backup folders by date

> WinSCP also supports drag-and-drop downloads, so you can easily pull individual backup files to your local machine when needed.

### Save Backups to Azure Blob Storage

#### Step 1: Create a Storage Account (if needed)

1. Log in to the [Azure Portal](https://portal.azure.com)
2. Search for **Storage accounts** and click **Create**
3. Choose your subscription and resource group
4. Enter a unique storage account name (e.g., `mycompanybackups`)
5. Select your region and performance tier (Standard is fine)
6. Click **Review + create**, then **Create**

#### Step 2: Create a Container

1. Open your storage account
2. Go to **Containers** (under Data storage)
3. Click **+ Container**
4. Enter a name (e.g., `arcgis-backups`)
5. Set access level to **Private**
6. Click **Create**

#### Step 3: Generate a Connection String

**Option A: Full Connection String (simpler)**

1. In your storage account, go to **Access keys** (under Security + networking)
2. Click **Show** next to the connection string
3. Copy the entire connection string

**Option B: SAS Token (more secure, time-limited)**

1. In your storage account, go to **Shared access signature**
2. Set permissions: **Read, Write, Delete, List, Add, Create**
3. Set allowed services: **Blob**
4. Set allowed resource types: **Container, Object**
5. Set expiration date (recommend 1-2 years for scheduled backups)
6. Click **Generate SAS and connection string**
7. Copy the **Connection string** (not just the SAS token)

#### Step 4: Configure Backup Utility

1. In the Backup Utility, select **Azure Blob** as storage type
2. Enter:
   - **Container name**: The container you created (e.g., `arcgis-backups`)
   - **Connection string**: Paste the full connection string

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">SAS tokens expire.</strong> Set a long expiration or create a calendar reminder to renew. If Azure uploads suddenly fail, check token expiration.

</div>

> **Tip**: Use [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) to browse and manage your backup blobs.

### Zip Before Upload

Enabled by default. Disable to preserve folder structure in cloud storage.

### Delete Local After Upload

Backups are saved locally first, then uploaded. Check this to remove the local copy after successful upload.

### Box, Dropbox, OneDrive, and Google Drive

These services sync local folders automatically:

1. Install the desktop sync app:
   - [Box Drive](https://www.box.com/resources/downloads)
   - [Dropbox](https://www.dropbox.com/install)
   - [OneDrive](https://www.microsoft.com/en-us/microsoft-365/onedrive/download) (included with Windows)
   - [Google Drive for Desktop](https://www.google.com/drive/download/)
2. Set your backup **Save Directory** to a folder inside the synced location:
   - Box: `C:\Users\YourName\Box\Backups`
   - Dropbox: `C:\Users\YourName\Dropbox\Backups`
   - OneDrive: `C:\Users\YourName\OneDrive\Backups`
   - Google Drive: `G:\My Drive\Backups` (or your mapped drive letter)

> **Tip**: Create a dedicated subfolder for backups.

---

## Email Notifications

Get notified when scheduled backups complete or fail.

### What's in the Email

- Contains the full Results.txt content in monospace formatting
- Subject line includes the organization name and backup status
- Failed backups use the subject prefix **"Backup failure"**
- When a software update is available, the subject includes **[UPDATE AVAILABLE]**

### Setup

In schedule settings, choose:
   - **CivicLens** - Uses the CivicLens mail server; no SMTP credentials needed, just enter recipient email addresses
   - **Custom SMTP** - Use your own mail server

### Custom SMTP

| Setting | Typical Value |
|---------|---------------|
| Server | smtp.yourcompany.com |
| Port | 587 (TLS) or 465 (SSL) |
| From | backups@yourcompany.com |

---

## Proxy Settings

If your network requires a proxy for internet access:

1. Expand the **Advanced** section on the Backup tab
2. Enter the proxy URL in the **Proxy** field
3. Format: `http://proxy.yourcompany.com:8080`

For authenticated proxies, use: `http://username:password@proxy.yourcompany.com:8080`

> **Note**: Used for all ArcGIS API connections. Leave empty for direct connection.

---

## Automatic Cleanup

Control backup folder growth:

1. In schedule settings, set **Delete after** to the number of days to keep backups
2. Optionally enable **Keep Monthly** to preserve one backup per month

### Test Before Deleting

Use **Dry Run** to preview deletions before enabling actual cleanup.

> **Safety**: The utility never deletes from system folders or drive roots.

---

## Tips and Best Practices

### Performance

- **Large organizations (10,000+ items)**: Use Deep Scan instead of Quick Scan
- **Slow exports**: Reduce concurrent threads (5-10 conservative, 10-20 recommended, 20-50 aggressive; default is 15)
- **Timeouts**: Increase the timeout for large feature services with many attachments
- **Unstable network**: Failed exports are retried automatically, but a stable connection helps
- **Split large backups**: Use filters (tag, group, folder, owner) to split large orgs into multiple jobs
- **Portal for ArcGIS**: Ensure sufficient disk space for temporary FGDB exports

### Security

All credentials (ArcGIS passwords, OAuth tokens, S3 keys, Azure connection strings, SMTP credentials) are stored in **Windows Credential Manager**, encrypted at rest using **Windows DPAPI**. Never written to config files, logs, or the registry. Each schedule uses a unique keyring entry.

The application communicates only with your ArcGIS environment. Exceptions:

- **License validation** - downloads a license list, checks your org name locally (not transmitted). Cached 30 days.
- **Update check** - checks for newer versions. No user data transmitted.
- **CivicLens email** (opt-in) - result summaries sent through CivicLens mail server. Use "Custom SMTP" to avoid this.

No telemetry or analytics.

- Use **Windows Credential Manager** for passwords (see [above](#windows-credential-manager-recommended-for-automation))
- Use dedicated service accounts for scheduled backups
- The executable is **code-signed** with a CivicLens LLC EV certificate

### Organization

- Use consistent **folder prefixes** to identify backups (e.g., `PROD_BACKUP`, `DEV_BACKUP`)
- Set up **separate schedules** for different orgs or content sets

### 7-Zip Recommendation

[7-Zip](https://www.7-zip.org/download.html) handles large backup archives better than Windows' built-in extractor.

---

## Understanding Your Backup

Each backup creates a timestamped folder using the **Filename Prefix** (default `BACKUP`), e.g., `BACKUP_20240115_143022`. Change the prefix to distinguish jobs (e.g., `PROD`, `DEV`). **Sort By** controls folder structure:

| Sort Option | Structure | Example Path |
|-------------|-----------|-------------|
| **Item Type Only** (default) | `Type/item` | `Feature Service/Roads_abc123.zip` |
| **Owner - Item Type** | `Owner/Type/item` | `jsmith/Feature Service/Roads_abc123.zip` |
| **Owner - Folder - Item Type** | `Owner/Folder/Type/item` | `jsmith/MyFolder/Feature Service/Roads_abc123.zip` |

With the default "Item Type Only" sorting:

```
BACKUP_20240115_143022/
├── Feature Service/
│   ├── Roads_abc123def456.zip          (File Geodatabase)
│   └── Parcels_def456abc123.zip
├── Web Map/
│   ├── City Map_aaa111bbb222.json      (Item configuration)
│   └── City Map_aaa111bbb222.zip       (Item resources: thumbnails, images)
├── Dashboard/
│   ├── Operations_ccc333ddd444.json
│   └── Operations_ccc333ddd444.zip
├── Inventory.csv
├── Results.txt
└── Full_Log.log
```

- **JSON files** - item configuration (web apps, maps, dashboards)
- **ZIP files** - File Geodatabase (feature services) or item resources (web apps/maps/dashboards)

Each item type folder also contains subfolders for supplemental metadata when "Get Definitions" is enabled:

- **Descriptions/** - item description JSON (`_desc.json`) and relationship metadata (`_relationships.json`) for items that have relationships (e.g., Map2Service, Survey2Service)
- **Thumbnails/** - item thumbnail images
- **Definitions/** - Feature Service admin definitions (`_def.json`, `_data.json`)

### Inventory.csv

Spreadsheet of every item discovered. Columns:

| Column | Description |
|--------|-------------|
| Item ID | Unique ArcGIS item identifier |
| Title | Item name |
| Owner | Item owner username |
| Type | ArcGIS item type (Feature Service, Web Map, etc.) |
| Folder | Owner's content folder |
| Created / Modified | Timestamps |
| Sharing | Private, Organization, or Public |
| Size | Item size in bytes |
| Result | Backup outcome (exported, failed, skipped, etc.) |
| Dependencies | Items that reference this item (if **Write Dependencies** is enabled) |

### Results.txt

Summary report:

- **BACKUP RESULTS** - Overall status, duration, and item counts
- **SUMMARY** - Totals for exported, failed, skipped, and excluded items
- **EXPORTED ITEMS BY TYPE** - Successful exports grouped by item type
- **FAILED ITEMS** - Items that could not be exported, grouped by failure reason with troubleshooting suggestions
- **PARTIAL EXPORTS** - Items that exported but with warnings
- **FALLBACK EXPORTS** - Feature Services exported as Shapefile or CSV because File Geodatabase export failed
- **EXPORTED WITHOUT ATTACHMENTS** - Feature Services re-exported without attachments due to blob errors
- **WARNINGS** - Non-fatal issues encountered during the backup
- **EXCLUDED ITEMS** - Items skipped due to filters or unsupported types

### Full_Log.log

Detailed log of the entire backup process. Include this when contacting support.

### StartErrorLog.txt

Located at `%LOCALAPPDATA%\CivicLens\BackupUtility\StartErrorLog.txt`. If the application fails to start, a native Windows dialog displays the error with system diagnostics (OS version, architecture, VC++ runtime status, Qt plugin availability) and saves it to this file. Send this file to support@civiclens.com for troubleshooting.

---

## Restoring Items

The **Restore** tab restores backed-up items to ArcGIS Online or Portal for ArcGIS.

### Supported Item Types

**JSON-based items** - full item data and resource restore:
- Web Map, Web Scene, Dashboard, StoryMap, Web Mapping Application
- Web Experience, QuickCapture Project
- Hub Site Application, Hub Page, Site Application, Site Page
- GeoBIM Project, GeoBIM Application

**Feature Service** - configuration restore only (no feature data):
- Symbology, popups, labels, visibility
- Service settings (capabilities, maxRecordCount, editor tracking)
- Domains and field properties
- Item metadata (title, tags, description, thumbnail)

**Feature Layer View** - configuration restore, plus "Create as new" support:
- Same configuration options as Feature Service (symbology, settings, domains, metadata)
- Create as new: recreates a view on a source Feature Service you specify

### Restore Modes

**Restore to existing item** - Overwrites an existing portal item with backed-up data, resources, and/or metadata. You provide the target item ID.

**Create as new item** - Creates a new portal item from backup files, named `{original_title}_restored_{timestamp}`. Useful for testing before overwriting production items, or recovering deleted items.

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">"Create as new item" is not available for regular Feature Services</strong> because this tool restores configuration only (symbology, settings, domains). To create a new Feature Service, publish the backed-up File Geodatabase, then use this tool to restore configuration to it. <strong>Feature Layer Views</strong> do support "Create as new" - see <a href="#/USER_GUIDE?id=restoring-feature-layer-views">Restoring Feature Layer Views</a>.

</div>

### Cross-Portal Restore

In **Create as new item** mode, you can create items on a different portal from where the backup originated. This is useful for migrating content between organizations.

1. Switch to **Create as new item** mode
2. Check **Create on a different portal**
3. Enter the target portal connection details (organization URL, username, password or OAuth)
4. Select a backup item and click **Restore** - the new item is created on the target portal

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Both portals must be licensed.</strong> The source portal (Connection group) and the target portal must each have a valid Backup Utility license. Licenses are validated before authentication is attempted. Short-term migration licenses are available at reduced cost for the target portal - contact <a href="mailto:info@civiclens.com">info@civiclens.com</a> for details.

</div>

After creating items on the target portal, use the **Find & Replace** tab to update internal item ID references (web map layer sources, dashboard data sources, etc.) so they point to the correct items in the new organization.

### Restoring JSON-Based Items

1. Open the **Restore** tab
2. Enter your portal connection details and click **Authorize**
3. Choose your restore mode: **Restore to existing item** or **Create as new item**
4. Click **Browse** and select a backup folder containing item files (e.g., `BACKUP_2026-01-15/Web Map/`)
5. Select an item from the dropdown - the item ID auto-fills
6. If restoring to an existing item, click **Validate** to confirm the item exists and you have edit permission
7. Choose restore options:
   - **Restore item data (JSON)** - replaces the item's JSON configuration (checked by default)
   - **Restore metadata** - updates title, tags, description, and thumbnail
   - **Create backup before restoring** - saves the current item state for rollback (checked by default, not shown in create-as-new mode)
8. Click **Restore**

Item resources are restored automatically when item data is restored - existing resources are replaced with the backup's `_resources.zip` contents.

<div style="border: 2px solid #2e7d32; border-left: 6px solid #2e7d32; background-color: #e8f5e9; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #2e7d32;">Field Maps configurations are included.</strong> Custom Field Maps configurations (offline areas, form layout, geofences, etc.) are stored as web map item resources. When you restore a web map with its resources, the Field Maps configuration is restored along with it - no extra steps needed.

</div>

### Restoring Feature Service Configuration

Feature Service restore updates service configuration from definition files. It does **not** modify feature data (rows or geometry).

1. Open the **Restore** tab and connect to your portal
2. Click **Browse** and select a backup folder - Feature Services are auto-detected from `Definitions/` subfolders
3. Select a Feature Service item from the dropdown (shown with "(Feature Service)" suffix)
4. Click **Validate** to confirm the service exists and has a valid URL
5. Choose what to restore:
   - **Symbology & Popups** - drawing info, popup config, labels (from `_data.json`)
   - **Domains & Field Properties** - coded value/range domains, field aliases (from `_def.json`)
   - **Restore metadata** - title, tags, description, thumbnail (from `_desc.json`)
6. Click **Restore**

> **Tip**: The Symbology & Popups option is only available when a `_data.json` file exists for the service.

### Restoring Feature Layer Views

Feature Layer Views (HFLVs) are backed up to the `HFLV Defs/` folder and appear in the item dropdown with a **(Feature Layer View)** suffix. Views support both restore modes.

**Restore to existing view** works the same as Feature Service restore - select the view, validate the target item ID, choose your options, and click Restore. The tool warns if the backup and target don't match (e.g., restoring a view backup to a regular Feature Service).

**Create as new view** recreates the view from scratch on a source Feature Service you specify:

1. Select a Feature Layer View item from the dropdown
2. Switch to **Create as new item** mode
3. Enter the **Source Feature Service ID** - the 32-character item ID of the hosted Feature Service this view should be built on
4. Choose what to apply: symbology, domains, metadata
5. Click **Restore**

The tool validates the source Feature Service before proceeding:
- The source must exist and have a valid service URL
- The source must be a hosted Feature Service (not another view)
- The source must contain the same layer/table IDs referenced in the backup

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Where to find the source Feature Service ID:</strong> Open the source Feature Service in your portal and copy the 32-character item ID from the URL bar (the <code>id=</code> parameter). If the original source was deleted, you'll need to republish it first and use the new item ID.

</div>

> **Tip**: If the view creation fails mid-way (e.g., after layers are added but before symbology is applied), the partially created view is automatically deleted to avoid orphaned items.

### StoryMap Restore

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">StoryMaps require a manual publish step after restoring.</strong> Open the StoryMap builder and click **Publish** - the "View Story" link won't work until you do. The restore recreates draft and published data resources, but ArcGIS requires an explicit publish.

</div>

### Safety Backup

When "Create backup before restoring" is checked (default), the current item state is saved to a timestamped subfolder before any changes are made:

**JSON-based items** (Web Maps, Dashboards, StoryMaps, etc.):

```
{backup_folder}/_pre_restore_2026-02-14_17-57-48/
    ItemTitle_abc123def456.json
    abc123def456_resources.zip
    Descriptions/ItemTitle_abc123def456_desc.json
    Thumbnails/abc123def456.png
```

**Feature Services and Feature Layer Views:**

```
{backup_folder}/_pre_restore_2026-02-14_17-57-48/
    ServiceTitle_abc123def456_def.json
    ServiceTitle_abc123def456_data.json
    ServiceTitle_abc123def456_desc.json
```

If the safety backup fails, the restore is aborted and the item is not modified.

> The safety backup folder path is printed to the console log at the start of every restore. Copy it from there if you need it later.

#### How to Revert Using the Safety Backup

After a successful restore, two buttons become available in the bottom bar:

- **View Item** - opens the restored item in your browser so you can verify it looks correct
- **Undo Last Restore** - reverts the item to its pre-restore state in one step (requires "Create backup before restoring" to have been enabled)

Click **View Item** first to inspect the result. If something is wrong, click **Undo Last Restore** to roll back. The undo runs the same restore process in reverse, using the safety backup files.

Both buttons remain available until you close the tab, disconnect, or start a new restore. If you need to revert after closing, use the manual method below.

**Manual revert (after closing or reconnecting):**

1. Copy the `_pre_restore_*` folder path from the console log (e.g., `C:\Backups\MYORG_2026-02-14\\_pre_restore_2026-02-14_17-57-48`)
2. In the Restore tab, paste that path into the **Backup Folder** field (or click Browse and navigate to it)
3. The item will appear in the dropdown - select it
4. Enter the same **Target Item ID** (the item you want to revert)
5. Check the options you want to revert (data, metadata, etc.)
6. Uncheck "Create backup before restoring" - you're already restoring from a safety backup, so a second one is unnecessary
7. Click **Restore**

This overwrites the item with its pre-restore state, effectively undoing the previous restore.

<div style="border: 2px solid #2e7d32; border-left: 6px solid #2e7d32; background-color: #e8f5e9; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #2e7d32;">Recommended workflow:</strong>

<ul>
<li>Always leave "Create backup before restoring" checked</li>
<li>After restoring, click <strong>View Item</strong> to verify it looks correct</li>
<li>If the restored item looks wrong, click <strong>Undo Last Restore</strong> to roll back</li>
<li>Check the console log for any warnings during the restore</li>
</ul>

</div>

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Cloud source limitation:</strong> The "Create backup before restoring" option is not available when restoring from S3 or Azure, because there is no local folder to write the safety backup to. If you need a safety net when restoring from cloud storage, run a regular backup of the target item first.

</div>

### Restoring from Cloud Storage

If your backups are stored in Amazon S3 or Azure Blob Storage, you can restore directly from the cloud without downloading the full backup first. Only the small configuration files needed for restore (JSON, thumbnails, descriptions - typically kilobytes) are downloaded on demand.

#### Step 1: Select Backup Source

At the top of the Restore tab, change **Backup Source** from **Local Folder** to **Amazon S3** or **Azure Blob**.

#### Step 2: Enter Cloud Credentials

**Amazon S3:**

| Field | What to Enter |
|-------|---------------|
| **Bucket** | Your S3 bucket name (e.g., `mycompany-arcgis-backups`) |
| **Subdirectory** | Optional path prefix within the bucket (e.g., `prod/weekly`) |
| **Access Key** | Your AWS Access Key ID |
| **Secret Key** | Your AWS Secret Access Key |

> **Tip**: Leave credentials blank if running on EC2/ECS with an IAM role attached, or if you have environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`) or `~/.aws/credentials` configured.

**Azure Blob:**

| Field | What to Enter |
|-------|---------------|
| **Container** | Your Azure blob container name (e.g., `arcgis-backups`) |
| **Connection String** | Your Azure Storage connection string (from Access keys or Shared access signature) |

#### Step 3: Connect and Select a Backup

Click **Connect**. The utility lists all folder-based backups in the cloud container/bucket.

Select a backup from the **Backup** dropdown. The item type and item dropdowns populate automatically, just like local folder restore.

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Zipped cloud backups are not supported for on-demand restore.</strong> If backups were uploaded as ZIP archives (the "Zip before upload" option), you must download the ZIP from your cloud console, extract it to a local folder, and use the Local Folder source instead.

</div>

#### Step 4: Restore as Normal

From here, the restore workflow is identical to local folder restore - select an item, choose your options, and click **Restore**. The needed files are downloaded to a temporary directory automatically before the restore begins.

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">"Create backup before restoring" is not available when restoring from cloud storage</strong> because there is no local folder to write the safety backup to. If you need a safety net, manually back up the item first using the Backup tab.

</div>

#### Cloud Credential Storage

Bucket name, subdirectory, access key ID, and container name are saved in application settings. Secret keys and connection strings are stored securely in **Windows Credential Manager** - the same approach used by the Backup tab.

#### Required Cloud Permissions

**S3**: `s3:ListBucket` and `s3:GetObject` on the backup bucket. Write permissions are not needed for restore.

**Azure**: Read and List permissions on the blob container. The connection string or SAS token must include these permissions.

### Restore Connection

The Restore tab has its own portal connection, separate from the Backup tab. You can restore to any item you have edit access to.

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Cross-portal limitations:</strong> JSON-based items contain internal references to other item IDs (layer sources, widget configs). These must exist on the target portal or the restored item will have broken references, effectively limiting JSON restores to the same portal. Use <a href="#/USER_GUIDE?id=find-and-replace">Find &amp; Replace</a> to remap old IDs after restoring. Feature Service config restore has no such limitation.

</div>

### What Happens on Failure

- **Safety backup failure**: The restore is aborted and the item is not modified.
- **Data restore failure**: The item may be in a partially updated state. Use the safety backup to [revert to the original](#how-to-revert-using-the-safety-backup).
- **Resource upload failure**: Individual resource failures are logged as warnings; remaining resources continue uploading.
- **Feature Service layer failure**: Each layer is updated independently. If one layer fails, the remaining layers are still processed. A summary of successes and failures is shown at the end.
- **Create-as-new failure**: If item creation succeeds but subsequent steps fail, the orphaned item is automatically deleted to avoid clutter. This applies to both JSON-based items and Feature Layer Views.

### Dependency Checking (Create as New)

When creating a new item, the tool scans the backup JSON for references to other item IDs and reports which dependencies were found. Missing dependencies on the target portal will result in broken references. Review the dependency list in the console before confirming.

### Relationship Restore

If a backed-up item has a `_relationships.json` file (created automatically during backup for items with relationships), a dialog appears after a successful restore offering to recreate those relationships.

**Forward relationships** (e.g., a web map's Map2Service link to a feature service) can be recreated if the target item exists on the portal. Each relationship has a checkbox so you can select which ones to restore.

**Reverse relationships** (owned by other items pointing to this one) are shown for reference but cannot be recreated from this side - you would need to restore or re-backup the other item.

> Relationship restore is available for same-portal restores only. For cross-portal migrations, relationships must be recreated manually or by restoring the origin items on the target portal.

---

<a id="find-and-replace"></a>

## Find & Replace

The **Find & Replace** tab scans your portal for references to old item IDs, service URLs, or other strings and replaces them with new values. Changes are applied one item at a time with full undo capability.

### When to Use Find & Replace

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">This tool modifies live portal items.</strong> Only JSON configuration of maps, apps, and dashboards is affected. Feature Service data (rows and geometry) is never touched.

</div>

**Republished Feature Service** - A Feature Service was republished with a new item ID and URL. Dependent maps and apps still reference the old service.

**Portal migration** - Content moved between organizations. Item IDs changed and internal references need remapping.

**Organization rename or server move** - Org short name changed (e.g., `oldorg.maps.arcgis.com` to `neworg.maps.arcgis.com`) or Portal moved to a new domain.

**Service folder restructure** - Services were reorganized into different server folders (e.g., `/rest/services/OldFolder/MyService/FeatureServer` to `/rest/services/NewFolder/MyService/FeatureServer`).

### Scannable Item Types

Find & Replace scans JSON-based item types:

- Web Map, Web Scene
- Dashboard
- Web Mapping Application
- StoryMap
- Web Experience, Web Experience Template
- QuickCapture Project
- Hub Site Application, Hub Page, Hub Initiative, Hub Initiative Template
- Site Application, Site Page
- GeoBIM Project, GeoBIM Application

### What Gets Scanned

For each item, two things are checked:

1. **Item text** - the JSON configuration property (the data behind Web Maps, Dashboards, etc.)
2. **JSON resources** - configuration files attached to the item (under 10 MB each)

Feature Service rows, geometry, attachments, and non-JSON resources are never read or modified.

### Step 1: Connect to Your Portal

Enter your portal connection details. This connection is independent of the Backup tab.

| Field | What to Enter |
|-------|---------------|
| **Organization** | Your org name (e.g., `myorg`) or full Portal URL (e.g., `https://gis.mycompany.com/portal`) |
| **Username** | Your ArcGIS username |
| **Password** | Your password, or leave blank and check **SSO / OAuth** for browser-based login (SSO, SAML, MFA) |

Check **Remember** to save your password securely in Windows Credential Manager.

Click **Authorize** to connect. You need edit permissions on the items you want to modify.

### Step 2: Enter Find/Replace Pairs

Add one or more find/replace pairs. **Auto-detection** categorizes each pair:

| Find String Pattern | Detection | Validation |
|---|---|---|
| 32-character hex string (e.g., `a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6`) | **Item ID** | Replace must also be 32-character hex |
| Starts with `http://` or `https://` | **URL** | Replace must also be a URL |
| Anything else | **General** | Allowed, but a confirmation prompt appears before scanning |

> **Tip**: For a republished Feature Service, you typically need two pairs: one for the old item ID mapped to the new item ID, and one for the old service URL mapped to the new service URL.

Up to 20 pairs per scan. Empty pairs and identical find/replace strings are ignored.

### Step 3: Set Scope Filters

Control which items are included in the scan:

- **Item types** - check the types you want to scan (all are selected by default). Use **Select All** / **Select None** for quick toggling.
- **Owner** - filter to items owned by a specific user (admin only), or leave blank to scan all
- **Mode** - **All org items (admin)** scans the entire organization (requires admin role). **My items only** scans your own items plus items in shared update groups where you have edit access.
- **Item IDs** - optionally enter specific item IDs (comma-separated) to scan only those items

### Step 4: Scan the Portal

Click **Scan Portal**. The scan is read-only - nothing is modified. Progress appears in the console.

Results show one row per affected item with:

- Item title, type, and owner
- Number of matches found
- An action button: **Apply**, **Undo**, or **Re-Apply**

Click a row to see match details in the lower panel:

| Column | Description |
|--------|-------------|
| **Location** | Where the match was found. `text` means the item's main JSON data. A filename like `config/config.json` means an item resource. |
| **Path** | The JSON path to the matched value (e.g., `operationalLayers[0].itemId` or `widgets[3].config.webmaps[0]`). Use this to understand what the reference controls. |
| **Context** | A snippet of the matched value with surrounding characters, so you can verify the match is what you expect before applying. |

### Step 5: Apply Changes

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">Changes modify live portal items.</strong> Apply to one item first, verify it in your browser, then proceed to others. Use Undo immediately if something looks wrong.

</div>

Click **Apply** on an item. Before making changes, the tool:

1. **Backs up** the current item text and resources locally
2. **Re-fetches** the item (not cached) to detect concurrent edits
3. **Verifies** find strings still present - aborts if the item changed since the scan
4. **Applies** replacements and **confirms** success

Click **Open** in the results table to verify the item in your browser.

### Step 6: Verify and Undo

After applying, the button changes to **Undo**. Click it to revert from the safety backup. After undoing, the button becomes **Re-Apply**.

Once confident, click **Apply All Remaining** to process the rest. Each item still gets its own backup, and you can undo individually.

> **StoryMaps:** Changes are saved as a draft. After applying, open the StoryMap builder and click **Publish** for changes to appear on the live version. The same applies after undoing.

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">Undo is session-based.</strong> Closing the tab, disconnecting, or closing the application clears undo history. Verify changes before closing.

</div>

### Safety Features

<div style="border: 2px solid #2e7d32; border-left: 6px solid #2e7d32; background-color: #e8f5e9; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #2e7d32;">Built-in safeguards:</strong>

<ul>
<li><strong>Read-only scan</strong> - no writes during the scan phase</li>
<li><strong>Safety backup before every change</strong> - if the backup fails, the item is NOT modified</li>
<li><strong>One at a time</strong> - you see and approve each item individually</li>
<li><strong>Undo</strong> - revert any applied change from the safety backup</li>
<li><strong>Stale data check</strong> - re-fetches item text before applying; aborts if the item has changed since the scan</li>
<li><strong>Cancellation</strong> - scan and apply can be cancelled at any time</li>
</ul>

</div>

Safety backups are saved in `%TEMP%\find_replace_backups`. Do not rely on them for long-term recovery - Windows may clean up temp files.

---

## Troubleshooting

### Application Fails to Start

If the application won't launch, check `%LOCALAPPDATA%\CivicLens\BackupUtility\StartErrorLog.txt` for a detailed error report including OS version, architecture, and DLL availability.

**Common causes:**

- **Older Windows version**: The StartErrorLog shows `ImportError: DLL load failed while importing QtWidgets`. This means the Qt UI framework requires Windows APIs not available on your OS version. Download the latest release from [civiclens.com/downloads](https://civiclens.com/downloads), which includes Qt compatibility fixes for older Windows versions. If the issue persists, contact support@civiclens.com with your StartErrorLog.
- **Missing VC++ Runtime**: Install the [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe) (x64).
- **Corrupted extraction**: Delete `_MEI*` folders in `%TEMP%` and try again. Antivirus software occasionally quarantines DLLs during extraction.

### "Connection failed"

- Check your organization name is spelled correctly
- Verify you have network access to ArcGIS
- Check if a proxy is required for your network

### "Authentication failed"

- Double-check your username - it's case-sensitive
- For SSO organizations, leave password blank and sign in via the browser popup
- Check if your account is locked or password expired

### "Export timeout"

- Increase the timeout setting (Backup Options section on the Backup tab)
- Reduce concurrent threads
- Large services may need longer timeouts

### "Memory error"

- Reduce concurrent threads to 5 or lower
- Close memory-heavy applications (browsers, GIS software)
- Break up the backup using owner or group filters

### Items Show as "Skipped"

Common reasons items are skipped:
- **Referenced/Registered Services**: Data lives in another org or external server
- **Multipatch Feature Services**: 3D geometry not supported by ArcGIS Online/Portal export API
- **Tile layers over 500,000 tiles**: Exceeds the ArcGIS Online export limit
- **Empty feature services**: Skipped if you chose to exclude empty services

### Application Appears to Be Hanging

Large feature services with attachments can take over an hour to prepare server-side. Other items continue exporting.

### ZIP File Won't Extract

Use [7-Zip](https://www.7-zip.org/download.html) instead of Windows' built-in extractor. Long path names can cause issues - move to a shorter path if needed.

### 'TEMP_FOR_EXPORT' Files Left in Portal

Temporary export files from interrupted backups are cleaned up automatically on the next run (files older than 72 hours).

### Incremental Backup Grabs Extra Services

ArcGIS treats settings changes (editing, sync, change tracking) as modifications, so incremental backups may include services without feature edits.

> **Note**: Incremental backups are only available for ArcGIS Online. Portal does not provide required edit timestamps.

### Schedule Shows "Not Available"

- Verify the Windows Task Scheduler service is running
- Confirm your Windows account has permission to create scheduled tasks

### "OAuth token expired"

- Open the schedule in the **Schedules** tab and click **Authorize...** to re-authenticate
- Tokens expire after approximately 2 weeks of inactivity

### Backup Runs but Exports 0 Items

- Check your filter settings - tags, owner, folders, or groups may be excluding everything
- Verify the account has access to content (non-admin accounts only back up items they own)

### Email Not Received

- Check your spam/junk folder
- Verify SMTP settings (server, port, credentials)
- Try the **CivicLens** email option first to rule out SMTP configuration issues

---

## Getting Help

**CivicLens Support**

- Email: support@civiclens.com
- Website: [civiclens.com](https://civiclens.com)
- Downloads: [civiclens.com/downloads](https://civiclens.com/downloads)

---

*Backup Utility for ArcGIS Online and Portal for ArcGIS v5.1.5*
*Copyright CivicLens*
