# Backup Utility for ArcGIS Online and Portal for ArcGIS

## User Guide | Version 5.0

---

## Quick Start

### Step 1: Download & Launch

1. Download **BackupUtility.exe** from [civiclens.com/downloads](https://civiclens.com/downloads)
2. Save it anywhere on your computer
3. Double-click to launch

### Step 2: Connect to Your Organization

| Field | What to Enter |
|-------|---------------|
| **Portal URL/Organization** | Your org name (e.g., `myorg` for myorg.maps.arcgis.com) |
| **Username** | Your ArcGIS username (case-sensitive!) |
| **Password** | Your password, or leave blank for SSO |

### Step 3: Choose Where to Save

Click **Browse** and select a folder for your backups.

> **Warning**: Don't select a drive root like `C:\` or system folders.

### Step 4: Start Backup

Click **Start Backup** and watch the progress. When complete, your backup folder will contain all your content.

---

## System Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended)
- Network access to ArcGIS Online or Portal

---

## What Gets Backed Up

### Feature Data
Exported as **File Geodatabase (.gdb)** with domains, attachments, and related tables:
- Feature Service

### Cached Tile Services
Exported as **Tile Package (.tpk or .vtpk)**:
- Map Service, Vector Tile Service, Map Image Layer

### Web Apps, Maps & Dashboards
Exported as **JSON configuration with item resources**:
- Web Map, Web Scene, Dashboard
- StoryMap, Web Experience, Web Mapping Application
- Hub Site Application, Hub Page, Site Application, Site Page
- QuickCapture Project, Workforce Project, Survey123 Form
- Insights Workbook, Insights Page, Insights Model, Insights Theme
- Hub Initiative, Solution, Mission
- Feature Collection, Feature Collection Template
- 360 VR Experience, Oriented Imagery Catalog

### Documents & Packages
Exported as **original uploaded files**:
- **Office**: Microsoft Word, Excel, PowerPoint, Visio, PDF
- **Apple**: iWork Keynote, Pages, Numbers
- **Images**: Image, Image Collection, Photos With Locations
- **GIS Packages**: File Geodatabase, Shapefile, GeoPackage, GeoJSON, KML, KML Collection, CSV, CSV Collection, GML
- **Pro/Desktop**: ArcGIS Pro Map, Project Package, Project Template, Map Package, Map Template, Layout, Layer Package, Scene Package, Scene Layer Package, Mobile Map Package, Mobile Basemap Package, Mobile Scene Package
- **Tiles**: Tile Package, Vector Tile Package, Compact Tile Package
- **Other Packages**: Locator Package, Geoprocessing Package, Rule Package, Deep Learning Package, Export Package, Task File
- **Notebooks**: Jupyter Notebook, Notebook, Notebook Code Snippet Library
- **Add-ins**: Desktop Add In, Explorer Add In, ArcGIS Pro Add In, Survey123 Add In, Dashboards Add In, AppBuilder Widget Package, Experience Builder Widget Package
- **Other**: Service Definition, CAD Drawing, Urban Model, Code Sample, Native Application, Pro Report, and 30+ more file types

### Not Exportable

These items are automatically skipped or have limited export:

**Reference-only (description saved, no data):**
- WMS, WMTS, WFS, Geocoding Service, Document Link

**Not exportable from hosted environment:**
- Scene Service, Scene Layer, Image Service

**Not included in backups:**
- Application (registered OAuth/API apps)

**Conditionally skipped during export:**
- Referenced Feature Services (data lives in another org or external server)
- Multipatch Feature Services (3D geometry not supported by export API)
- Tile layers exceeding 500,000 tiles (platform limit)
- Unpublished apps

### Item Types Reference

Use these exact type names for the **Exclude Types** filter.

#### Feature Services (exported as File Geodatabase)
`Feature Service`

#### Tile Services (exported as Tile Package)
`Map Service`, `Vector Tile Service`, `Map Image Layer`

#### Apps, Maps & Configuration Items (exported as JSON + resources)
`Web Map`, `Web Scene`, `Dashboard`, `StoryMap`, `Web Experience`, `Web Mapping Application`, `Hub Site Application`, `Hub Page`, `Site Application`, `Site Page`, `QuickCapture Project`, `Workforce Project`, `Insights Workbook`, `Insights Page`, `Insights Model`, `Insights Theme`, `Hub Initiative`, `Hub Initiative Template`, `Solution`, `Mission`, `Feature Collection`, `Feature Collection Template`, `360 VR Experience`, `Oriented Imagery Catalog`, `Investigation`, `Hub Event`, `GeoBIM Project`, `GeoBIM Application`, `Data Pipeline`, `Style`, `StoryMap Theme`, `Story Map Theme`, `Map Area`, `Symbol Set`, `Color Set`, `Content Category Set`, `Web Experience Template`, `Group Layer`, `Mobile Application`

#### Files & Packages (exported as original uploaded file)
`Form`, `Image`, `Image Collection`, `Photos With Locations`, `PDF`, `Microsoft Word`, `Microsoft Excel`, `Microsoft Powerpoint`, `Visio Document`, `iWork Keynote`, `iWork Pages`, `iWork Numbers`, `CSV`, `CSV Collection`, `Shapefile`, `File Geodatabase`, `GeoPackage`, `GeoJson`, `KML`, `KML Collection`, `GML`, `Table`, `CAD Drawing`, `Apache Parquet`

#### ArcGIS Pro/Desktop Packages (exported as original file)
`ArcGIS Pro Map`, `Project Package`, `Project Template`, `Map Package`, `Map Template`, `Layout`, `Layer`, `Layer Package`, `Scene Package`, `Scene Layer Package`, `Mobile Map Package`, `Mobile Basemap Package`, `Mobile Scene Package`, `Tile Package`, `Vector Tile Package`, `Compact Tile Package`, `Locator Package`, `Geoprocessing Package`, `Geoprocessing Package (Pro version)`, `Geoprocessing Sample`, `Rule Package`, `Deep Learning Package`, `Export Package`, `Task File`, `ArcPad Package`, `Explorer Map`, `Globe Document`, `Windows Mobile Package`, `Explorer Layer`, `Pro Report`, `Desktop Style`, `Raster function template`, `ArcGIS Pro Configuration`, `Workflow Manager (Classic) Package`, `Statistical Data Collection`, `SQLite Geodatabase`

#### Notebooks & Code (exported as original file)
`Jupyter Notebook`, `Notebook`, `Notebook Code Snippet Library`, `Code Sample`, `Code Attachment`

#### Add-ins & Extensions (exported as original file)
`Desktop Add In`, `Explorer Add In`, `ArcGIS Pro Add In`, `Survey123 Add In`, `Dashboards Add In`, `AppBuilder Widget Package`, `Experience Builder Widget`, `Experience Builder Widget Package`

#### Other Exportable Types
`Service Definition`, `Map Service Definition`, `Report Template`, `Map Document`, `Insights Workbook Package`, `Urban Model`, `CityEngine Web Scene`, `Native Application`, `Native Application Template`, `Native Application Installer`, `Desktop Application`, `Desktop Application Template`, `Administrative Report`

#### Automatically Skipped (Reference-Only)
These items reference external services and contain no exportable data. Item metadata/description is saved:
`WMS`, `WMTS`, `WFS`, `Geocoding Service`, `Document Link`

#### Automatically Skipped (Not Exportable)
These cannot be exported from the hosted ArcGIS environment:
`Scene Service`, `Scene Layer`, `Image Service`

#### Not Included in Backups
`Application` (registered OAuth/API apps - these are configuration registrations, not content)

Each backup includes an **Inventory.csv** listing every item with its ID, title, owner, type, dates, and backup result.

---

## Logging In

### Regular Password Login

Enter your username and password. Note: **Username is case-sensitive**.

### SSO / Multi-Factor Authentication

1. Leave the password field **empty**
2. A browser window opens
3. Complete your organization's login
4. Done—the app receives your credentials automatically

> **Tip**: For scheduled backups with SSO, use the OAuth option in schedule settings.

### OAuth for Scheduled Backups

For organizations using SSO/SAML/MFA, OAuth allows scheduled backups to run without storing passwords:

1. In the **Schedules** tab, check **SSO / OAuth**
2. Click **Authorize...**
3. Complete your organization's login in the browser
4. The utility stores a refresh token securely in Windows Credential Manager
5. Scheduled backups will automatically refresh the token as needed

> **Note**: If your organization changes SSO providers or policies, you may need to re-authorize.

### Windows Credential Manager (Recommended for Automation)

Store passwords securely instead of typing them:

1. Open **Windows Credential Manager** (Control Panel > Credential Manager)
2. Click **Add a generic credential**
3. Enter:
   - **Internet or network address**: `CIVICLENS`
   - **User name**: Your ArcGIS username
   - **Password**: Your password
4. In the Backup Utility, enter `wcm` as your password

> **Security Tip**: This is the recommended approach for scheduled backups—passwords are stored securely by Windows and never saved in plain text.

### Admin vs. Non-Admin Mode

The utility automatically detects your administrator status:

- **Admin Mode**: Can back up all organization content (all users' items)
- **Non-Admin Mode**: Can only back up items you own or items shared with you

If you're not an organization administrator, the utility automatically switches to non-admin mode. You'll see a message like:

```
username is not an administrator for orgname. Running in non-admin mode.
```

This is normal for non-admin users and the backup will proceed with accessible content.

---

## Filtering What to Back Up

### Full vs. Incremental Backup

- **Full Backup**: Everything in your organization
- **Incremental Backup**: Only items modified within a specified number of days

Set **Incremental Days** to back up only recent changes (e.g., `7` for the last week).

> **Note**: Incremental backup only works for ArcGIS Online, not Portal. The utility automatically adds a 12-hour buffer to ensure items near the time boundary are not missed.

### Filter by Item Type

Choose from:
- **All Items** — Everything
- **All Items Except Cached Tile Layers** — Skip large tile caches
- **Feature Services Only** — Just feature data

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

> **Note**: Groups and Folders filters are mutually exclusive—use one or the other.

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

Skip specific item types from backup. Enter type names separated by semicolons:

```
Web Map;Dashboard;StoryMap
```

This is useful when you only want to back up feature data without web apps.

### Advanced Options

Found in the **Advanced** section of the Backup tab:

| Option | Description |
|--------|-------------|
| **Service Definitions** | Download .sd files (publishing artifacts from ArcGIS Pro). Large files, often redundant if you have the original Pro project. |
| **Write Dependencies** | Analyze and record which items reference other items. Creates additional columns in Inventory.csv. Useful for migration planning. |
| **Empty Services** | Include feature services with zero features. Preserves the schema even if no data exists. Enabled by default. |
| **Tag Backed-Up Items** | Add a `last_backup` tag to items after successful backup. ⚠️ Updates the item's modified date, which affects incremental backups. |

---

## Scheduling Automatic Backups

### Create a Schedule

1. Go to the **Schedules** tab
2. Click **New Schedule**
3. Set the frequency (Daily, Weekly, or Monthly)
4. Enter your connection details
5. Click **Save**

The backup runs automatically via Windows Task Scheduler—no need to stay logged in.

> **Tip**: Set backups to run overnight when network usage is low.

### Email Confirmation

Configure email notifications to receive confirmation when backups complete or alerts if they fail.

### Managing Schedules

- **Edit**: Modify settings anytime
- **Delete**: Removes the schedule and Windows Task
- **Run Now**: Trigger an immediate backup
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
8. Copy the **Access Key ID** and **Secret Access Key** (save these securely—you won't see the secret again!)

#### Step 4: Configure Backup Utility

1. In the Backup Utility, select **Amazon S3** as storage type
2. Enter:
   - **Bucket name**: Your bucket name (e.g., `mycompany-arcgis-backups`)
   - **Subdirectory** (optional): A path within the bucket (e.g., `prod/weekly`)
   - **Access Key ID**: From the IAM user
   - **Secret Access Key**: From the IAM user

> **Tip**: Leave credentials blank if running on EC2/ECS with an IAM role attached—the utility will use instance credentials automatically.

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

> **Warning**: SAS tokens expire! Set a long expiration date or create a calendar reminder to renew before it expires. If backups suddenly fail, check if your SAS token has expired.

> **Tip**: Use [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) to browse and manage your backup blobs.

### Delete Local After Upload

When using cloud storage (S3 or Azure), backups are first saved locally then uploaded. By default, both copies are kept.

Check **Delete local after upload** to remove the local staging folder after successful cloud upload. This saves disk space but means you only have the cloud copy.

### Box, Dropbox, OneDrive, and Google Drive

These services sync local folders to the cloud automatically. To use them:

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

Your backups will automatically sync to the cloud after each run.

> **Tip**: Create a dedicated subfolder for backups to keep them organized.

---

## Email Notifications

Get notified when scheduled backups complete or fail.

### Setup

1. In your schedule settings, find Email options
2. Choose:
   - **CivicLens** — Uses our mail server (just enter recipient emails)
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

> **Note**: The proxy is used for all ArcGIS API connections. Leave empty for direct connection.

---

## Automatic Cleanup

Keep your backup folder from growing forever:

1. In schedule settings, set **Days to Retention**
2. Optionally enable **Keep Monthly Archives** to preserve one backup per month

### Test Before Deleting

Use **Dry Run** mode to preview what would be deleted without actually removing files. Check the logs to verify the cleanup logic before enabling actual deletion.

> **Safety**: The utility never deletes from system folders or drive roots.

---

## Tips and Best Practices

### Performance

- **Large organizations (10,000+ items)**: Use Deep Scan instead of Quick Scan
- **Slow exports**: Reduce concurrent threads from 15 to 5-10
- **Timeouts**: Increase the timeout for very large feature services with lots of attachments
- **Unstable network**: The utility retries failed exports automatically, but a stable connection helps
- **Split large backups**: Use tag, group, folder, or owner filters to break up very large orgs into multiple backup jobs
- **Portal for ArcGIS**: Ensure your server has available disk space for temporary FGDB exports during processing

### Security

- Use **Windows Credential Manager** for passwords (see above)
- Use dedicated service accounts for scheduled backups
- Store cloud credentials (S3/Azure) securely

### Organization

- Use consistent **folder prefixes** to identify backups (e.g., `PROD_BACKUP`, `DEV_BACKUP`)
- Set up **separate schedules** for different orgs or content sets
- **Spot-check your backups**: Open a few exported files to verify they contain data

### 7-Zip Recommendation

For extracting backup ZIP files, we recommend [7-Zip](https://www.7-zip.org/download.html) as it handles large archives better than Windows' built-in ZIP support.

---

## Understanding Your Backup

Each backup creates a timestamped folder like `BACKUP_20240115_143022` containing:

```
BACKUP_20240115_143022/
├── Feature Service/
│   ├── Roads.zip
│   └── Parcels.zip
├── Web Map/
│   └── City Map.json
├── Dashboard/
│   └── Operations.json
├── Inventory.csv
└── Results.txt
```

- **Folders by item type** — Feature Services, Web Maps, Dashboards, etc.
- **Inventory.csv** — Complete record of every item
- **Results.txt** — Summary with timing and success/failure counts

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

- Increase the timeout setting (Options tab)
- Reduce concurrent threads
- Large services with millions of features may need longer timeouts

### "Memory error"

- Reduce concurrent threads to 5 or lower
- Close memory-heavy applications (browsers, GIS software)
- Break up the backup using owner or group filters

### Items Show as "Skipped"

Common reasons items are skipped:
- **Referenced/Registered Services**: The data lives in another organization's ArcGIS Online or Portal, or on an external server—only the item metadata is in your org
- **Multipatch Feature Services**: 3D geometry cannot be exported via the ArcGIS REST API
- **Tile layers over 500,000 tiles**: Exceeds the platform export limit
- **Empty feature services**: Skipped if you chose to exclude empty services

### Application Appears to Be Hanging

If the application seems frozen, it's likely working on a large service in the background:

- Large feature services with attachments can take an hour or more to prepare on the server
- The process will move on to the next item after the timeout period, continuing to work on the large item in the background
- If it's showing "Checking and finishing any potential active exports…" at the end, it's actively downloading

> **Tip**: Don't click in the console window or select text—this can pause the output (a Windows feature). Press Escape or right-click to resume.

### ZIP File Won't Extract

- Use [7-Zip](https://www.7-zip.org/download.html) instead of Windows' built-in extractor
- Long path names can cause issues with Windows' default extractor
- If needed, move the backup to a shorter path before extracting

### 'TEMP_FOR_EXPORT' Files Left in Portal

If the backup was interrupted, temporary export files may remain. You can delete these manually, or they'll be cleaned up automatically during the next backup run (files older than 72 hours are removed).

### Incremental Backup Grabs Extra Services

Occasionally, incremental backups may include services that haven't had feature edits. This happens because ArcGIS considers settings changes (editing, sync, change tracking) to be edits.

> **Note**: Incremental backups work only with ArcGIS Online. Portal for ArcGIS does not expose the required properties.

---

## Getting Help

**CivicLens Support**

- Email: support@civiclens.com
- Website: [civiclens.com](https://civiclens.com)
- Downloads: [civiclens.com/downloads](https://civiclens.com/downloads)

---

*Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0*
*Copyright CivicLens*
