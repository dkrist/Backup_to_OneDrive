The **GoodSync History folder** (officially called `_gsdata\_history_`) is a hidden subfolder that GoodSync creates automatically on the **destination side** (Right side) of your job when you enable the **"Save deleted/replaced files to History folder, multiple versions"** option in the **Recycled/History** tab. This is the feature you're using for your NAS → OneDrive backup job to support long-term retention with multiple rollback points.

### Overall `_gsdata_` Folder Purpose
GoodSync places a hidden folder named `_gsdata_` at the root of the synced/backup folder (on OneDrive in your case, or on the 4 TB/2 TB drive for local jobs). Inside it:
- Synchronization state files (e.g., `_file_state._gs` — required for incremental tracking).
- Job logs (`.log` files).
- Versioning subfolders: `_saved_` (for single last-version Recycle Bin, which you're not using) and `_history_` (for multiple versions).

The `_gsdata_` folder is hidden by default (enable "Show hidden files" in File Explorer or onedrive.com browser view to see it).

### `_gsdata\_history_` Folder Structure & How Versions Are Stored
When GoodSync detects a file on the **source** (NAS) that is newer/modified, or when a file is deleted on the source (but deletion doesn't propagate due to your settings), it saves the **old/previous version** from the destination before overwriting or (effectively) "losing" it.

Key characteristics of the `_history_` folder:
- **Location**: Always on the **Right side** (destination) of the job — in your case, inside your OneDrive backup folder (e.g., `/Backups/NAS_Main_04-2024/_gsdata/_history_`).
- **Directory mirroring**: GoodSync **preserves the original folder structure** from your source (NAS "_Main_04-2024"). So if you have a file at `Documents/Projects/report.docx` on NAS, its versions appear at `_gsdata/_history_/Documents/Projects/report_YYYY-MM-DD_HH-MM-SS.docx`.
- **Filename format**: Each version gets a **timestamp suffix** added **before the extension** (ISO-like format, usually `YYYY-MM-DD_HH-MM-SS` or similar, depending on exact version).
  - Example: Original file `photo.jpg` → overwritten versions become:
    - `photo_2026-03-21_14-30-45.jpg`
    - `photo_2026-03-20_09-15-22.jpg`
    - etc.
  - Deleted files are also placed here with their timestamped name (so you can recover them).
- **No subfolders inside `_history_` for metadata**: It's flat per original path — just timestamped file copies mirroring your folder tree.
- **Storage behavior**:
  - A **new version** is saved **every time** GoodSync overwrites/replaces a file on the destination (e.g., during each sync when NAS has a newer copy).
  - Versions are kept **indefinitely** unless you enable **Cleanup _history_ folder after this many days** (your current setting of 90 days purges versions older than that; we discussed unchecking or extending to 365+ for better retention).
  - Space usage: Only changed files create versions — with your 48.3 GB data and personal use (low churn), growth is slow even over years.

### Example Structure (Illustrated)
Assume your NAS folder looks like this:
```
_Main_04-2024/
├── Documents/
│   └── report.docx
└── Photos/
    └── vacation.jpg
```

After several overwrites/deletes and syncs, the OneDrive side might look like:
```
NAS_Main_04-2024/                  ← Your main backup folder
├── Documents/
│   └── report.docx                ← Latest version from NAS
├── Photos/
│   └── vacation.jpg               ← Latest
└── _gsdata/                       ← Hidden root folder
    ├── _file_state._gs            ← State tracking (don't touch)
    ├── some_job.log               ← Logs
    └── _history_/                 ← Where multiple versions live
        ├── Documents/
        │   ├── report_2026-03-21_10-45-00.docx   ← Older version 1
        │   └── report_2026-03-15_16-20-30.docx   ← Older version 2
        └── Photos/
            └── vacation_2026-03-10_08-05-12.jpg  ← Deleted/old version
```

### Practical Tips for Your Setup
- To view: On onedrive.com → go to your backup folder → enable "Show hidden files" (or use the OneDrive desktop app temporarily to browse, then disable sync if unwanted).
- To recover: Browse `_gsdata/_history_`, copy the desired timestamped file back to the main folder (rename by removing the timestamp), or copy to NAS.
- If `_gsdata_` causes issues on OneDrive (rare): In Job Options → Right Side tab → check **"No _gsdata_ folder here"** — this moves state/version metadata to your local PC's GoodSync profile folder, but keeps the actual timestamped versions on OneDrive.
- For local jobs (e.g., to 4 TB WD): Same structure appears directly on the drive (easier to browse in File Explorer).

This structure keeps your backups versioned and recoverable without cluttering the main folders — ideal for your long-term retention focus. If you test a few changes and want help interpreting what you see in `_history_`, share details (no sensitive paths/files), and I can guide recovery steps! Next could be the WD drive job setup if ready.