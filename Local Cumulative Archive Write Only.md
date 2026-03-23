# Copy 2 – Local Cumulative Archive (Write-Only Style) – Final Recommendations
**Purpose**: One-way, cumulative backup of the "_Main_04-2024" shared folder (48.3 GB) from Synology DS423 NAS to a local 4 TB WD HDD, creating a growing archive that never deletes files even when deletions occur on the NAS.  
**Goal**: Long-term data retention with multiple version history; deletions on source are ignored; ideal for preserving everything forever (e.g., family photos, documents, future videos).  
**Tool**: GoodSync (latest version per user screenshots)  
**Job Name**: NAS_Main_to_4TB_Archive (or similar)  
**Job Type**: Backup – Left to Right (1-way)  
**Destination Drive**: 4 TB WD internal HDD (SATA or USB enclosure for rotation)

## Core Settings

### General Tab
- Job Type: **Backup Left → Right (1-way)**  
- Propagate Deletions: **OFF / No**  
  → Critical: Files deleted on NAS **stay forever** on the 4 TB drive (cumulative archive behavior)  
- Ignore file time in destination: Optional (leave unchecked unless timestamp mismatches occur)

### Recycled/History Tab (Versioning)
- Save deleted/replaced files to Recycle Bin, last version only: **Unchecked**  
- Save deleted/replaced files to History folder, multiple versions: **Checked**  
  → Enables timestamped versions in hidden `_gsdata\_history_` folder on the 4 TB drive  
  → Preserves multiple rollback points (stronger for long-term retention than single-version Recycle Bin)  
- Cleanup _history_ folder after this many days:  
  **Recommended: Uncheck** (keep versions indefinitely – safe with 48 GB data and 4 TB capacity)  
  **Alternative**: Check and set to 730 (2 years) or 1095 (3 years) if you prefer eventual cleanup  
- Cleanup _saved_ folder after this many days: Irrelevant (Recycle Bin off), can leave as default

### Auto Tab (Scheduling)
- Recommended:  
  - **On Timer** → Weekly (e.g., every Sunday at 3:00 AM) or Monthly (e.g., 1st of month)  
  - OR leave unchecked for **manual runs** (connect drive → run job → disconnect/store)  
- Delay: Default (20–60 seconds)  
- Note: Manual or infrequent schedule fits best for air-gap-like rotation (drive disconnected most of the time)

### Left Side (Source)
- Path: Network share to Synology NAS → "_Main_04-2024" folder  
  (e.g., \\NAS-IP\Main_04-2024 or mapped drive Z:)

### Right Side (Destination)
- Connection: Local drive letter (e.g., E: or F:)  
- Folder: Dedicated folder on 4 TB drive, e.g.  
  `E:\Archive_Main_04-2024` or `E:\NAS_Archive`  
- `_gsdata\_history_` will appear here automatically (enable "Show hidden files" to view)

### Additional Recommended Options
- Filters tab: (optional) Exclude temp files (e.g., ~* , *.tmp) or include only specific types if desired  
- Speed/Limits: No throttling needed (local SATA/USB is fast)  
- Errors/Conflicts: Auto-rename or ask on conflict (default fine)  
- Script/Email: Enable email notifications for failures (optional but useful)

## Rotation with 2 TB Drive (Optional Secondary Archive)
- Alternate between 4 TB and 2 TB drives (e.g., 4 TB for current/weekly, 2 TB for monthly/deeper history)  
- Create a second job (or clone this one): NAS → 2 TB drive folder (e.g., F:\Archive_Secondary)  
- Use identical settings (no deletion propagation, multiple versions, indefinite cleanup)  
- Workflow:  
  1. Connect current drive  
  2. Run job (manual or scheduled)  
  3. Disconnect and store safely (drawer, fireproof safe, off-site)  
  4. Swap to other drive next cycle

## Testing & Verification Checklist
1. Connect 4 TB drive → Run **Analyze** → Confirm only expected copy/overwrite actions  
2. Run **Sync** → Initial copy (48 GB will be fast locally)  
3. After sync:  
   - Open File Explorer → 4 TB drive → enable "Show hidden files"  
   - Look for `_gsdata\_history_` folder with timestamped versions  
4. Test deletion/overwrite:  
   - Modify or delete a test file on NAS  
   - Run Sync  
   - Confirm old version remains in `_gsdata\_history_` on 4 TB drive  
5. Recovery test: Copy desired timestamped file from `_history_` back to main folder or NAS (remove timestamp suffix)

## Why This Configuration Fits Your Needs
- **Cumulative / write-only archive** → Nothing is ever automatically deleted from the 4 TB drive  
- **Multiple versions (History mode)** → Excellent long-term retention (recover from various points in time)  
- **No deletion propagation** → Protects against accidental mass deletes on NAS  
- **Small data size (48.3 GB)** → 4 TB drive stays mostly empty for years of growth  
- **Rotation support** → 2 TB drive adds extra historical depth and protection  
- **Local & fast** → Quick restores when drive is connected; complements OneDrive (off-site)  

Last updated: March 2026 (aligned with user's GoodSync version and 3-2-1 backup strategy)