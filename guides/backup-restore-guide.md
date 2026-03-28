# Backup & Restore Guide

A practical guide for backing up and restoring data in a Windows environment, covering both local and server-side scenarios.

---

## 🗂️ What to Back Up

| Item | Priority |
|---|---|
| User profile data (`C:\Users\username`) | High |
| Desktop, Documents, Downloads, Pictures | High |
| Outlook PST / OST files | High |
| Browser bookmarks | Medium |
| Application settings | Medium |
| System state (servers) | High |

---

## 💾 Backup Methods

### 1. Windows Backup (Built-in)

**File History** — automatic backup of user files to external drive or network location.

1. Open **Settings > System > Storage > Advanced storage settings > Backup options**
2. Add a drive (external or network)
3. Set backup frequency and retention period
4. Click **Back up now**

> Best for: personal user file backups on workstations

---

**Windows Server Backup** — for server environments.

Install the feature first:
```powershell
Install-WindowsFeature Windows-Server-Backup
```

Backup system state:
```powershell
wbadmin start systemstatebackup -backuptarget:E:
```

Backup specific volume:
```powershell
wbadmin start backup -backuptarget:E: -include:C: -allCritical -quiet
```

---

### 2. Robocopy (Command Line)

Good for scripted or scheduled file backups.

```cmd
robocopy "C:\Users\username\Documents" "D:\Backup\Documents" /MIR /LOG:C:\backup.log
```

| Flag | Meaning |
|---|---|
| `/MIR` | Mirror source to destination (deletes removed files) |
| `/COPY:DAT` | Copy data, attributes, timestamps |
| `/LOG:path` | Write log to file |
| `/XA:H` | Exclude hidden files |
| `/R:3` | Retry 3 times on failure |

Schedule with Task Scheduler for automatic backups.

---

### 3. OneDrive / Microsoft 365 Backup

1. Open **OneDrive settings > Sync and backup > Manage backup**
2. Enable backup for Desktop, Documents, and Pictures
3. Files sync automatically to the cloud

> Useful for roaming users or hybrid environments

---

## 🔁 Restore Procedures

### Restore from File History

1. Open **Control Panel > File History**
2. Click **Restore personal files**
3. Browse to the date/version you need
4. Click the green **Restore** button

---

### Restore from Windows Server Backup

Restore files:
```powershell
wbadmin start recovery -version:MM/DD/YYYY-HH:MM -itemType:File -items:"C:\folder" -recursive -recoveryTarget:"D:\Restore"
```

Restore system state:
```powershell
wbadmin start systemstaterecovery -version:MM/DD/YYYY-HH:MM
```

List available backups:
```powershell
wbadmin get versions
```

---

### Restore from Robocopy Backup

```cmd
robocopy "D:\Backup\Documents" "C:\Users\username\Documents" /MIR
```

> ⚠️ `/MIR` will delete files at the destination that don't exist in the source. Double-check before running.

---

### Restore Previous Versions (Shadow Copy)

1. Right-click the folder or file > **Properties**
2. Go to the **Previous Versions** tab
3. Select a version and click **Restore** or **Open** to browse

> Requires Shadow Copy / Volume Snapshot Service (VSS) to be enabled on the volume.

---

## ✅ Backup Checklist

- [ ] Backup destination confirmed (external drive / network share / cloud)
- [ ] User data folders included (Desktop, Documents, Downloads, Pictures)
- [ ] Outlook data backed up (.pst file if applicable)
- [ ] Backup log reviewed for errors
- [ ] Test restore performed to confirm backup integrity
- [ ] Backup schedule set and verified

---

## 🛠️ Troubleshooting

| Issue | Fix |
|---|---|
| File History not backing up | Check drive connection, review Event Viewer > File History |
| VSS / Previous Versions missing | Enable Shadow Copies on the volume via Disk Management |
| Robocopy access denied errors | Run as administrator or check NTFS permissions |
| wbadmin backup fails | Check disk space on target, review `wbadmin get status` |
| OneDrive sync stuck | Sign out and back in, or reset OneDrive: `%localappdata%\Microsoft\OneDrive\onedrive.exe /reset` |

---

*Environment: Windows Server 2019/2022, Windows 11 clients*
