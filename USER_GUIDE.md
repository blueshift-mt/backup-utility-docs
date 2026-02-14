# Backup Utility for ArcGIS Online and Portal for ArcGIS

## User Guide | Version 5.0

---

## Quick Start (On-Demand Backup)

See [Scheduling Automatic Backups](#scheduling-automatic-backups) for information on scheduling backups.

### Step 1: Download & Launch

1. Download **BackupUtility.exe** from [civiclens.com/downloads](https://civiclens.com/downloads)
2. Save it anywhere on your computer
3. Double-click to launch

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Note for parameters file users:</strong> The application no longer auto-detects parameters files. To run a headless backup with a parameters file, use <code>BackupUtility.exe --file parameters.xlsx</code>. Parameters file support is temporary and will be removed in a future release — please migrate to the built-in Schedules tab.

</div>

### Step 2: Connect to Your Organization

| Field | What to Enter |
|-------|---------------|
| **Portal URL/Organization** | **ArcGIS Online:** Your org name (e.g., `myorg`) or full URL (e.g., `myorg.maps.arcgis.com`). **Portal:** Your Portal URL (e.g., `https://gis.mycompany.com/portal`) |
| **Username** | Your ArcGIS username (case-sensitive) |
| **Password** | Your password, or leave blank and check SSO/OAuth for single sign-on |

### Step 3: Choose Where to Save

Click **Browse** to select a backup folder. Local paths (e.g., `D:\Backups`) and UNC paths (e.g., `\\server\share\backups`) are supported.

> **Warning**: Don't select a drive root like `C:\` or network share roots like `\\server\share`.

### Step 4: Start Backup

Click **Start Backup**. The Progress tab shows real-time status as items are discovered, exported, and saved. Results.txt and Inventory.csv are generated on completion.

---

## System Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended)
- Network access to ArcGIS Online or Portal

---

## What Gets Backed Up

### Hosted Feature and Table Data
Exported as **File Geodatabase (.gdb)** with domains, attachments, and related tables:
- Feature Service

### Location Tracking Services
Exported as **Shapefile (.shp)** to a `Location Tracking` folder. Cannot be exported to File Geodatabase.

### Tile & Map Services
Exported as **Tile Package (.tpk or .vtpk)**:
- Map Service, Vector Tile Service

### Web Apps, Maps & Dashboards
Exported as **JSON configuration with item resources**:
- **Maps**: Web Map, Web Scene (Field Maps configurations are included in web map item resources)
- **Apps**: Dashboard, StoryMap, Web Experience, Web Mapping Application, Mobile Application
- **Field Apps**: QuickCapture Project, Workforce Project
- **Insights**: Insights Workbook, Insights Page, Insights Model, Insights Theme
- **Hub/Sites**: Hub Site Application, Hub Page, Hub Initiative, Hub Initiative Template, Hub Event, Site Application, Site Page
- **Other**: Solution, Mission, Investigation, GeoBIM Project, GeoBIM Application, Data Pipeline, Style, StoryMap Theme, Map Area, Symbol Set, Color Set, Content Category Set, Web Experience Template, Group Layer, Feature Collection, Feature Collection Template, 360 VR Experience, Oriented Imagery Catalog

### Survey123 Forms
Downloaded as **Survey123 survey packages** including XLSForm, media, pulldata files, CSVs, and JSON configuration:
- Form

### Documents
Downloaded in **original format**:
- **Office**: Microsoft Word, Microsoft Excel, Microsoft Powerpoint, Visio Document, PDF
- **Apple**: iWork Keynote, iWork Pages, iWork Numbers
- **Images**: Image, Image Collection, Photos With Locations
- **Other**: CAD Drawing, Report Template, Administrative Report

### GIS Data Files
Downloaded in **original format**:
- Shapefile, File Geodatabase, GeoPackage, GeoJson, KML, KML Collection, GML, Apache Parquet, SQLite Geodatabase

### Tabular Data
Downloaded in **original format**:
- CSV, CSV Collection

### ArcGIS Packages
Downloaded in **original format**:
- **Project**: Project Package, Project Template, ArcGIS Pro Map, ArcGIS Pro Configuration
- **Map/Layer**: Map Package, Map Template, Layer, Layer Package, Map Document
- **Scene**: Scene Package, Scene Layer Package, Mobile Scene Package
- **Mobile**: Mobile Map Package, Mobile Basemap Package
- **Tile**: Tile Package, Vector Tile Package, Compact Tile Package
- **Analysis**: Locator Package, Geoprocessing Package, Geoprocessing Package (Pro version), Geoprocessing Sample
- **Other**: Rule Package, Deep Learning Package, Export Package, Task File, Layout, Pro Report, Desktop Style, Raster function template, Workflow Manager (Classic) Package, Statistical Data Collection, Insights Workbook Package
- **Legacy**: ArcPad Package, Explorer Map, Explorer Layer, Globe Document, Windows Mobile Package

### Service Definitions
Downloaded in **original format**:
- Service Definition

### Add-ins
Downloaded in **original format**:
- Desktop Add In, Explorer Add In, ArcGIS Pro Add In, Survey123 Add In, Dashboards Add In, AppBuilder Widget Package, Experience Builder Widget, Experience Builder Widget Package

### Other
Downloaded in **original format**:
- Urban Project, CityEngine Web Scene, Native Application, Native Application Template, Native Application Installer, Desktop Application, Desktop Application Template, Code Sample, Code Attachment

### Notebooks
Downloaded as **.ipynb files**:
- Notebook, Notebook Code Snippet Library

### Not Exportable

Automatically skipped or limited:

**Reference-only (description saved, no data):**
- WMS, WMTS, WFS, Geocoding Service, Document Link

**Not exportable from hosted environment:**
- Scene Service, Scene Layer, Image Service

**Not included in backups:**
- Application (registered OAuth/API apps)

**Conditionally skipped during export:**
- Referenced Feature Services (data lives in another org or external server)
- Multipatch Feature Services (3D geometry not supported by export API)
- Tile layers exceeding the platform's export limit
- Unpublished apps

### Item Types Reference

Use these exact type names for the **Exclude Types** filter.

#### Feature Services (exported as File Geodatabase)
`Feature Service`

#### Tile Services (exported as Tile Package)
`Map Service`, `Vector Tile Service`

#### Survey123 Forms (downloaded as survey package)
`Form`

#### Apps, Maps & Configuration Items (exported as JSON + resources)
`Web Map`, `Web Scene`, `Dashboard`, `StoryMap`, `Web Experience`, `Web Mapping Application`, `Hub Site Application`, `Hub Page`, `Site Application`, `Site Page`, `QuickCapture Project`, `Workforce Project`, `Insights Workbook`, `Insights Page`, `Insights Model`, `Insights Theme`, `Hub Initiative`, `Hub Initiative Template`, `Solution`, `Mission`, `Feature Collection`, `Feature Collection Template`, `360 VR Experience`, `Oriented Imagery Catalog`, `Investigation`, `Hub Event`, `GeoBIM Project`, `GeoBIM Application`, `Data Pipeline`, `Style`, `StoryMap Theme`, `Story Map Theme`, `Map Area`, `Symbol Set`, `Color Set`, `Content Category Set`, `Web Experience Template`, `Group Layer`, `Mobile Application`

#### Files & Packages (downloaded in original format)
`Image`, `Image Collection`, `Photos With Locations`, `PDF`, `Microsoft Word`, `Microsoft Excel`, `Microsoft Powerpoint`, `Visio Document`, `iWork Keynote`, `iWork Pages`, `iWork Numbers`, `CSV`, `CSV Collection`, `Shapefile`, `File Geodatabase`, `GeoPackage`, `GeoJson`, `KML`, `KML Collection`, `GML`, `CAD Drawing`, `Apache Parquet`, `Report Template`

#### ArcGIS Pro/Desktop Packages (downloaded in original format)
`ArcGIS Pro Map`, `Project Package`, `Project Template`, `Map Package`, `Map Template`, `Layout`, `Layer`, `Layer Package`, `Scene Package`, `Scene Layer Package`, `Mobile Map Package`, `Mobile Basemap Package`, `Mobile Scene Package`, `Tile Package`, `Vector Tile Package`, `Compact Tile Package`, `Locator Package`, `Geoprocessing Package`, `Geoprocessing Package (Pro version)`, `Geoprocessing Sample`, `Rule Package`, `Deep Learning Package`, `Export Package`, `Task File`, `ArcPad Package`, `Explorer Map`, `Globe Document`, `Windows Mobile Package`, `Explorer Layer`, `Pro Report`, `Desktop Style`, `Raster function template`, `ArcGIS Pro Configuration`, `Workflow Manager (Classic) Package`, `Statistical Data Collection`, `SQLite Geodatabase`

#### Notebooks (downloaded as .ipynb)
`Notebook`, `Notebook Code Snippet Library`

#### Code (downloaded if file exists)
`Code Sample`, `Code Attachment`

These items may not have an associated file. If none exists, only item metadata is saved.

#### Add-ins & Extensions (downloaded in original format)
`Desktop Add In`, `Explorer Add In`, `ArcGIS Pro Add In`, `Survey123 Add In`, `Dashboards Add In`, `AppBuilder Widget Package`, `Experience Builder Widget`, `Experience Builder Widget Package`

#### Other Exportable Types
`Service Definition`, `Map Service Definition`, `Map Document`, `Insights Workbook Package`, `Urban Project`, `CityEngine Web Scene`, `Native Application`, `Native Application Template`, `Native Application Installer`, `Desktop Application`, `Desktop Application Template`, `Administrative Report`

#### Automatically Skipped (Reference-Only)
These items reference external services and contain no exportable data. Item metadata/description is saved:
`WMS`, `WMTS`, `WFS`, `Geocoding Service`, `Document Link`

#### Automatically Skipped (Not Exportable)
These cannot be exported from the hosted ArcGIS environment:
`Scene Service`, `Scene Layer`, `Image Service`, `Map Image Layer`

#### Not Included in Backups
`Application` (registered OAuth/API apps - these are configuration registrations, not content)

Each backup includes an **Inventory.csv** listing every item with its ID, title, owner, type, dates, and backup result.

---

## Logging In

### Regular Password Login

Enter your username and password. **Username is case-sensitive.**

### SSO / Multi-Factor Authentication

For organizations using single sign-on or MFA-enforced login:

1. Leave the password field **empty**
2. Check the **SSO / OAuth** checkbox
3. Click **Start Backup** — a browser window opens
4. Complete your organization's login
5. The app receives your credentials and backup starts

### OAuth for Scheduled Backups

Scheduled backups run unattended and can't open a browser each time. OAuth uses a stored refresh token:

1. In the **Schedules** tab, check **SSO / OAuth**
2. Click **Authorize...**
3. Complete your organization's login in the browser
4. The utility stores a refresh token securely in Windows Credential Manager
5. Scheduled backups will automatically refresh the token as needed

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">Refresh tokens expire after approximately 2 weeks of inactivity.</strong> If no backup runs within that window, the schedule will fail until you re-authorize. Tokens also expire if your credentials change or your organization changes SSO providers.

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

<strong style="color: #2e7d32;">Security Tip:</strong> Recommended for scheduled backups. All credentials stored in Windows Credential Manager are encrypted at rest using Windows DPAPI and are only accessible to your Windows user account. Passwords are never stored in plain text. Credentials never leave your machine — CivicLens has no access to your stored passwords or tokens.

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

<strong style="color: #1565c0;">Note:</strong> Groups and Folders filters are mutually exclusive—use one or the other.

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
| **Service Definitions** | Download .sd files from ArcGIS Pro. |
| **Write Dependencies** | Record item-to-item dependencies in Inventory.csv. Useful for migration planning and restoration. |
| **Empty Services** | Include feature services with zero features to preserve schema. Enabled by default. |
| **Tag Backed-Up Items** | Tag items with `last_backup_<date>` after export (e.g., `last_backup_11FEB2026_14:30`). ⚠️ Updates the item's modified date, causing every tagged item to appear in the next incremental backup. Not recommended with incremental mode. |

---

## Scheduling Automatic Backups

### Create a Schedule

1. Go to the **Schedules** tab
2. Click **New Schedule**
3. Set the frequency (Daily, Weekly, or Monthly)
4. Enter your connection details
5. Click **Save**

Schedules run via Windows Task Scheduler in the background. No need to keep the app open or stay logged in.

#### Windows Service Account

Each schedule requires a **Windows Password** so Task Scheduler can run backups when no user is logged in. Optionally specify a **Windows User** to run the task under a different account (defaults to the current user).

> **Tip**: The password is stored securely by Windows Task Scheduler, not by the application.

<div style="border: 2px solid #1565c0; border-left: 6px solid #1565c0; background-color: #e3f2fd; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #1565c0;">Monitor Running Backups:</strong> Open the application at any time to check progress on the <strong>Progress</strong> tab. Closing the GUI does not stop the backup.

</div>

> **Tip**: Set backups to run overnight when network usage is low.

> **Overlap Detection**: If a backup is still running when the next scheduled run starts, a warning is logged to Results.txt. Both backups will run, but concurrent processes double the memory, disk, and network usage on your machine. You may want to increase the schedule interval or reduce backup scope.

### Where to Find Scheduled Tasks

The application creates tasks in Windows Task Scheduler under the **CivicLens** folder. To view them:

1. Open **Task Scheduler** (search "Task Scheduler" in the Start menu)
2. In the left panel, expand **Task Scheduler Library** and click **CivicLens**
3. Your backup tasks will be listed as `BackupUtility_<id>`

Useful for troubleshooting. In normal use, manage schedules from the **Schedules** tab.

### Email Confirmation

Receive notifications when backups complete or fail.

### Managing Schedules

- **Edit**: Modify settings anytime
- **Delete**: Removes the schedule and its Windows Task Scheduler task
- **Run Now**: Run the backup immediately through the GUI with progress monitoring
- **Enable/Disable**: Pause without deleting

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

<strong style="color: #2e7d32;">Tip:</strong> Leave credentials blank if running on EC2/ECS with an IAM role attached—the utility will use instance credentials automatically.

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

Enabled by default. The backup folder is zipped into a single archive before uploading. Disable to preserve folder structure and browse backups directly in cloud storage.

### Delete Local After Upload

Backups are saved locally first, then uploaded. Check **Delete local after upload** to remove the local copy after successful upload.

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
   - **CivicLens** — Uses the CivicLens mail server; no SMTP credentials needed, just enter recipient email addresses
   - **Custom SMTP** — Use your own mail server

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

All credentials are stored in **Windows Credential Manager**, encrypted at rest using **Windows DPAPI** (Data Protection API). This includes ArcGIS passwords, OAuth refresh tokens, S3 access keys, Azure connection strings, and SMTP credentials. Credentials are tied to your Windows user account and inaccessible to other users on the same machine. Credentials are never transmitted to CivicLens or any external service — they remain entirely on your machine.

No passwords or tokens are ever written to configuration files, the Windows registry, log files, or any other unprotected location. Each scheduled backup stores its credentials under a unique keyring entry.

The application communicates only with your ArcGIS Online or Portal for ArcGIS environment (and Esri-managed infrastructure that ArcGIS redirects to for downloads, such as AWS or Azure endpoints for exports, static items, and attachments). It does not phone home or collect usage data. The only exceptions are:

- **License validation** — downloads a license list and checks your organization name locally. Your org name is not transmitted. The list is cached locally for 30 days so the check is infrequent.
- **Update check** — a lightweight query to see if a newer version is available. No user data is transmitted.
- **CivicLens email** (opt-in only) — if you select the "CivicLens" email notification option, backup result summaries are sent through the CivicLens mail server. CivicLens can see the contents of these emails. To avoid this, select "Custom SMTP" and use your own mail server, or disable email notifications entirely.

No telemetry, analytics, or usage tracking of any kind is collected or transmitted.

- Use **Windows Credential Manager** for passwords (see [above](#windows-credential-manager-recommended-for-automation))
- Use dedicated service accounts for scheduled backups
- The executable is **code-signed** with a CivicLens LLC EV certificate — verify the digital signature if you receive it from a third party

### Organization

- Use consistent **folder prefixes** to identify backups (e.g., `PROD_BACKUP`, `DEV_BACKUP`)
- Set up **separate schedules** for different orgs or content sets

### 7-Zip Recommendation

[7-Zip](https://www.7-zip.org/download.html) handles large backup archives better than Windows' built-in extractor.

---

## Understanding Your Backup

Each backup creates a timestamped folder using the **Filename Prefix** (default `BACKUP`), e.g., `BACKUP_20240115_143022`. Change the prefix to distinguish between backup jobs (e.g., `PROD`, `DEV`). The **Sort By** setting controls the folder structure:

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

- **Folders by item type** — Feature Services, Web Maps, Dashboards, etc.
- **JSON files** — Item configuration (for web apps, maps, dashboards)
- **ZIP files with item ID** — For feature services: File Geodatabase. For web apps/maps/dashboards: a ZIP of item resources stored alongside the JSON file in the same folder

### Inventory.csv

A spreadsheet of every item discovered during the backup. Columns include:

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

Summary report organized into sections:

- **BACKUP RESULTS** — Overall status, duration, and item counts
- **SUMMARY** — Totals for exported, failed, skipped, and excluded items
- **EXPORTED ITEMS BY TYPE** — Successful exports grouped by item type
- **FAILED ITEMS** — Items that could not be exported, grouped by failure reason with troubleshooting suggestions
- **PARTIAL EXPORTS** — Items that exported but with warnings
- **FALLBACK EXPORTS** — Feature Services exported as Shapefile or CSV because File Geodatabase export failed
- **EXPORTED WITHOUT ATTACHMENTS** — Feature Services re-exported without attachments due to blob errors
- **WARNINGS** — Non-fatal issues encountered during the backup
- **EXCLUDED ITEMS** — Items skipped due to filters or unsupported types

### Full_Log.log

Detailed log of the entire backup process. Include this when contacting support.

### StartErrorLog.txt

Located next to the application executable. Records startup failures, configuration errors, and fatal crashes from scheduled backups where no console is visible. Check this file if a scheduled backup fails silently.

---

## Restoring Items

The **Restore** tab lets you restore backed-up items to ArcGIS Online or Portal for ArcGIS. Two restore modes are supported:

### Supported Item Types

**JSON-based items** — full item data and resource restore:
- Web Map, Dashboard, Web Experience, StoryMap, Web Mapping Application, QuickCapture Project, Hub Site/Page, Site Application/Page

**Feature Service** — configuration restore only (no feature data):
- Symbology, popups, labels, visibility
- Service settings (capabilities, maxRecordCount, editor tracking)
- Domains and field properties
- Item metadata (title, tags, description, thumbnail)

### Restoring JSON-Based Items

1. Open the **Restore** tab
2. Enter your portal connection details and connect
3. Click **Browse** and select a backup folder containing item files (e.g., `BACKUP_2026-01-15/Web Map/`)
4. Select an item from the dropdown — the item ID auto-fills
5. Click **Validate** to confirm the item exists and you have edit permission
6. Choose restore options:
   - **Restore item data (JSON)** — replaces the item's configuration
   - **Restore metadata** — updates title, tags, description, thumbnail
   - **Create backup before restoring** — saves current state for rollback
7. Click **Restore**

### Restoring Feature Service Configuration

Feature Service restore updates service configuration from the definition files captured during backup. It does **not** modify feature data (rows or geometry) — use ArcGIS Pro for data restoration.

1. Open the **Restore** tab and connect to your portal
2. Click **Browse** and select a backup folder — Feature Services are auto-detected from `Definitions/` subfolders
3. Select a Feature Service item from the dropdown (shown with "(Feature Service)" suffix)
4. Click **Validate** to confirm the service exists and has a valid URL
5. Choose what to restore:
   - **Symbology & Popups** — drawing info, popup config, labels (from `_data.json`)
   - **Service Settings** — capabilities, maxRecordCount, editor tracking (from `_def.json`)
   - **Domains & Field Properties** — coded value/range domains, field aliases (from `_def.json`)
   - **Restore metadata** — title, tags, description, thumbnail (from `_desc.json`)
6. Click **Restore**

<div style="border: 2px solid #d32f2f; border-left: 6px solid #d32f2f; background-color: #fdecea; padding: 16px 20px; border-radius: 4px; margin: 20px 0;">

<strong style="color: #d32f2f;">Service Settings warning:</strong> Changing capabilities (e.g., removing Query or Sync) can break dependent maps and apps. The restore process shows a warning before proceeding. Use the "Create backup before restoring" option so you can revert if needed.

</div>

> **Tip**: The Symbology & Popups option is only available when a `_data.json` file exists for the service (approximately 68% of Feature Services have this file).

### Restore Connection

The Restore tab uses an **independent** portal connection, separate from the Backup tab. This allows you to restore items to a different portal or organization than the one you backed up from.

---

## Troubleshooting

### "Connection failed"

- Check your organization name is spelled correctly
- Verify you have network access to ArcGIS
- Check if a proxy is required for your network

### "Authentication failed"

- Double-check your username—it's case-sensitive
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
- **Multipatch Feature Services**: 3D geometry not supported by export API
- **Tile layers over 500,000 tiles**: Exceeds the platform export limit
- **Empty feature services**: Skipped if you chose to exclude empty services

### Application Appears to Be Hanging

Large feature services with attachments can take over an hour to prepare server-side. Other items continue exporting in the meantime.

> **Tip**: Don't click in the console window or select text—this can pause the output (a Windows feature). Press Escape or right-click to resume.

### ZIP File Won't Extract

Use [7-Zip](https://www.7-zip.org/download.html) instead of Windows' built-in extractor. Long path names can cause issues — move to a shorter path if needed.

### 'TEMP_FOR_EXPORT' Files Left in Portal

Temporary export files from interrupted backups are cleaned up automatically on the next run (files older than 72 hours). You can also delete them manually.

### Incremental Backup Grabs Extra Services

Incremental backups may include services without feature edits. ArcGIS treats settings changes (editing, sync, change tracking) as modifications.

> **Note**: Incremental backups are only available for ArcGIS Online. Portal does not provide required edit timestamps.

### Schedule Shows "Not Available"

- Verify the Windows Task Scheduler service is running
- Confirm your Windows account has permission to create scheduled tasks

### "OAuth token expired"

- Open the schedule in the **Schedules** tab and click **Authorize...** to re-authenticate
- Tokens expire after approximately 2 weeks of inactivity

### Backup Runs but Exports 0 Items

- Check your filter settings — tags, owner, folders, or groups may be excluding everything
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

*Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0*
*Copyright CivicLens*
