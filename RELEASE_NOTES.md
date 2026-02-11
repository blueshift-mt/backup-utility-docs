# Backup Utility v5.0 Release Notes

**Release Date:** February 2026

We're excited to announce version 5.0 of the Backup Utility for ArcGIS Online and Portal for ArcGIS! This release brings a completely redesigned interface, smarter authentication handling, comprehensive documentation, and dozens of quality-of-life improvements based on your feedback.

---

## Highlights

### All-New Documentation Portal

Say goodbye to static PDFs! Our documentation now lives at **[docs.civiclens.com](https://docs.civiclens.com)** — a fast, searchable, always up-to-date resource featuring:

- **Complete Item Types Reference** — Every exportable item type, organized by export method, with accurate information about what gets backed up and why certain items are skipped
- **Step-by-step cloud storage guides** — Detailed walkthroughs for Amazon S3 and Azure Blob Storage setup, including IAM policies and lifecycle rules
- **Troubleshooting section** — Solutions for common issues like timeouts, memory errors, and skipped items
- **Quick Start guide** — Get your first backup running in under 5 minutes

### Smarter SSO/OAuth Authentication

We've completely rethought how OAuth and SSO authentication works:

- **Seamless on-demand SSO** — Just check the SSO/OAuth box, click Start Backup, and sign in through your browser. No separate authorization step needed.
- **Intelligent token management** — Change your organization URL? The app automatically detects this and prompts for re-authorization instead of failing silently.
- **Secure token storage** — Refresh tokens are stored in Windows Credential Manager, not in config files.
- **Clear status indicators** — Always know your authorization state with visual status labels.

### Enhanced Progress Monitoring

The Progress tab now gives you better insight into what's happening:

- **Admin/Non-Admin indicator** — See at a glance whether you're running with full organization access or limited to your own content
- **Accurate CPU reporting** — CPU percentage now matches what you see in Task Manager (normalized across all cores)
- **Real-time item tracking** — Watch items move through discovery, export, and download phases

---

## New Features

### Proxy Support
Enterprise users rejoice! Full HTTP/HTTPS proxy support is now available for organizations that require proxied internet access. Configure once in the Advanced section and all ArcGIS API connections route through your proxy.

### Centralized Message System
All user-facing messages are now managed through a central configuration, making it easier to:
- Review and customize error messages
- Maintain consistency across the application
- Support future localization efforts

### Improved Results Reporting

The Results.txt file generated after each backup has been overhauled:

```
ITEMS BY TYPE
  Feature Services:                    127
  Web Maps:                             45
  Dashboards:                           23
  ...
```

- **Clean alignment** — Type counts are now perfectly formatted for easy scanning
- **Enhanced failure details** — Failed items now include owner, direct URL to the item, and actionable suggestions
- **Clear categorization** — Partial exports, skipped items, and failures are clearly separated

---

## Improvements

### Reliability
- **Smarter retry logic** — The backup engine now better handles transient failures and connection issues
- **Graceful degradation** — If one export method fails, the system automatically tries alternatives before marking an item as failed

### User Experience
- **EULA dialog refresh** — Improved readability with better contrast and proper scrollbar styling
- **Consistent terminology** — "Incremental backup" is now used consistently throughout (previously mixed with "differential")
- **Grammar fixes** — "Back up" (verb) vs "backup" (noun) used correctly throughout the interface

### Documentation Accuracy
We audited every item type claim against the actual codebase:
- **Map Image Layer** correctly documented as non-exportable (it's a reference type, not a hosted service)
- **Reference-only items** (WMS, WMTS, WFS) clearly explained — metadata is saved, but there's no data to export
- **Scene Service, Scene Layer, Image Service** documented as platform limitations, not application limitations

---

## Technical Details

### What Gets Backed Up

| Category | Export Format | Examples |
|----------|--------------|----------|
| Feature Services | File Geodatabase (.gdb.zip) | Feature Service, with attachments, domains, and related tables |
| Tile Services | Tile Package (.tpk/.vtpk) | Map Service, Vector Tile Service |
| Apps & Maps | JSON + Resources | Web Map, Dashboard, StoryMap, Web Experience, Survey123 Form |
| Files & Packages | Original File | PDF, Shapefile, File Geodatabase, Project Package, Notebooks |

### Automatically Skipped

| Type | Reason |
|------|--------|
| WMS, WMTS, WFS, Geocoding Service | Reference-only — points to external services with no data in your org |
| Scene Service, Scene Layer, Image Service | Cannot be exported from hosted ArcGIS environment |
| Map Image Layer | Non-hosted reference service |
| Application | OAuth/API registrations, not content |

### System Requirements

- Windows 10/11 or Windows Server 2016+
- 4 GB RAM minimum (8 GB recommended for large organizations)
- Network access to ArcGIS Online or Portal for ArcGIS
- .NET Framework 4.7.2+ (for Windows Task Scheduler integration)

---

## Migration Notes

### From v4.x

- **No action required** — Your existing schedules and saved settings will continue to work
- **OAuth tokens preserved** — Previously authorized schedules remain authorized
- **Documentation links updated** — All in-app help links now point to docs.civiclens.com

### Licensing

Version 5.0 introduces our new subscription licensing model:
- **Existing perpetual licenses** are grandfathered and will continue to work
- **New purchases** are annual subscriptions with continuous updates and support
- **Maintenance renewals** ensure you always have access to the latest features

---

## Coming Soon

We're already working on the next release:
- **Attachment blob error recovery** — Automatic retry without attachments when server-side blob errors occur
- **Parallel organization backups** — Run multiple org backups simultaneously
- **Enhanced scheduling options** — More granular scheduling with exclusion windows

---

## Feedback

We build this tool for you. Tell us what's working and what isn't:

- **Email:** support@civiclens.com
- **Documentation:** [docs.civiclens.com](https://docs.civiclens.com)
- **Website:** [civiclens.com](https://civiclens.com)

Thank you for choosing CivicLens Backup Utility!

---

*Backup Utility for ArcGIS Online and Portal for ArcGIS v5.0*
*Copyright 2026 CivicLens*
