# NAS to OneDrive Backup – Final Recommended Configuration
**Purpose**: One-way backup of the "_Main_04-2024" shared folder (48.3 GB) from Synology DS423 NAS to Microsoft OneDrive 1 TB  
**Goal**: Long-term data retention with versioning; no deletion propagation; no interference with local C:\Users\...\OneDrive folder  
**Tool**: GoodSync (latest version as per user's screenshot)  
**Job Name**: NAS_Main-To-OneDrive_Backup  
**Job Type**: Backup – Left to Right (1-way)

## Core Settings

### General Tab
- Job Type: **Backup Left → Right (1-way)**  
- Propagate Deletions: **OFF / No**  
  → Deletions on NAS do **not** remove files from OneDrive (critical for retention)  
- Ignore file time in destination: Optional (leave unchecked unless timestamp issues appear)

### Recycled/History Tab (Versioning – Most Important)
- Save deleted/replaced files to Recycle Bin, last version only: **Unchecked**  
- Save deleted/replaced files to History folder, multiple versions: **Checked**  
  → Enables timestamped versions in hidden `_gsdata\_history_` folder on OneDrive  
  → Allows rollback to multiple points in time (stronger than single-version Recycle Bin)  
- Cleanup _saved_ folder after this many days: Checked at 90 days (ignored since Recycle Bin is off)  
- Cleanup _history_ folder after this many days:  
  **Recommended: Uncheck** (keep versions indefinitely)  
  **Alternative (if you prefer controlled growth):** Check and set to 365 or 730 days

### Auto Tab (Scheduling)
- Recommended:  
  - **On Timer** → Daily at 2:00 AM (or off-peak time)  
  - OR **On File Change** → Analyze and Sync (real-time-ish for frequently changing files)  
- Delay: Default (20–60 seconds)

### Left Side (Source)
- Path: Network share to Synology NAS → "_Main_04-2024" folder  
  (e.g., \\NAS-IP\Main_04-2024 or via WebDAV/SMB mapping)

### Right Side (Destination)
- Connection: **OneDrive (Microsoft Graph / Office365)** – direct cloud API (not local OneDrive folder)  
- Folder: Create/use a dedicated folder, e.g.  
  `/Backups/NAS_Main_04-2024` or `/Archive/Main_04-2024`  
- **Important**: Do **not** select or point to C:\Users\...\OneDrive – job uses cloud-only connection

### Additional Recommended Options
- Filters tab: (optional) Include only *.docx, *.jpg, *.pdf, etc. if you want to exclude temp files  
- Speed/Limits: No throttle needed (48 GB is small; let it use full upload speed)  
- Errors/Conflicts: Default (ask on conflict or auto-rename)  
- No _gsdata_ folder here (Right Side tab): Check **only if** `_gsdata\_history_` causes upload errors on OneDrive (rare)

## Testing & Verification Checklist
1. Run **Analyze** → Confirm only expected copy/overwrite actions  
2. Run **Sync** → Initial upload (may take hours depending on internet)  
3. After sync:  
   - Log in to onedrive.com → navigate to your backup folder  
   - Enable "Show hidden files" → look for `_gsdata\_history_` folder  
   - Verify timestamped versions appear after file changes  
4. Test deletion/overwrite:  
   - Modify or delete a test file on NAS  
   - Run Sync  
   - Confirm old version remains in `_gsdata\_history_` on OneDrive  
5. Recovery test: Copy older version from OneDrive back to NAS or local folder

## Why This Configuration Fits Your Needs
- **One-way only** → No risk of OneDrive changes affecting NAS or local PC  
- **No deletion propagation** → Files deleted on NAS stay in OneDrive forever  
- **Multiple versions (History mode)** → Best long-term retention (rollback to various points in time)  
- **Small data size (48.3 GB)** → Versioning overhead is negligible on 1 TB OneDrive  
- **Direct cloud connection** → Zero interaction with local C:\Users\...\OneDrive folder  
- **Cleanup optional** → Indefinite keep is safe and recommended for personal archives

Last updated: March 2026 (based on user's GoodSync version and screenshot)