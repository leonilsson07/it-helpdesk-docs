# User Profile Corrupted – Troubleshooting Guide

A step-by-step guide for diagnosing and fixing corrupted Windows user profiles.

---

## 🔍 Symptoms of a Corrupted Profile

- User logs in to a **temporary profile** ("You've been signed in with a temporary profile")
- Desktop is empty, settings are reset, pinned apps are gone
- Error at login: *"The User Profile Service failed the sign-in"*
- Profile loads very slowly or partially
- Files appear missing from Desktop/Documents

---

## ⚠️ Before You Start

- **Do not delete the old profile folder yet** — user data may still be recoverable inside it
- Back up `C:\Users\username` to a safe location before making changes
- Identify whether this is a **local profile** or a **roaming profile** (domain environment)

---

## 🛠️ Fix 1: Repair via Registry (Most Common Fix)

This fixes the *"temporary profile"* issue caused by a registry key problem.

1. Log in as a **different admin account** (not the affected user)
2. Open **Registry Editor**: `regedit`
3. Navigate to:
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList
```
4. Look for two keys that match the affected user's SID — one ending in `.bak` and one without
5. **If there are two keys (one with `.bak`)**:
   - Delete the key **without** `.bak`
   - Rename the `.bak` key by removing the `.bak` suffix
   - Double-click **State** in that key and set the value to `0`
6. **If there is only one key with `.bak`**:
   - Rename it by removing `.bak`
   - Set **State** value to `0`
7. Reboot and log in as the affected user

---

## 🛠️ Fix 2: Create a New Profile and Migrate Data

Use this when the registry fix does not work or the profile is too corrupted to repair.

### Step 1 – Create a new local user profile
1. Log in as admin
2. Go to **Settings > Accounts > Other users > Add account**
3. Create a new local account (or have the user log in once to generate a new profile)
4. Log out and log back in as admin

### Step 2 – Copy data from the old profile
Navigate to `C:\Users\` and copy the following folders from the old profile to the new one:

| Folder | Notes |
|---|---|
| `Desktop` | User's desktop files |
| `Documents` | User documents |
| `Downloads` | Downloaded files |
| `Pictures` | Images |
| `AppData\Roaming\Microsoft\Signatures` | Outlook email signatures |
| `AppData\Roaming\Microsoft\Outlook` | PST/OST files (if not on Exchange) |
| `AppData\Roaming\Microsoft\Sticky Notes` | Sticky Notes data |
| `Favorites` | IE/Edge favorites (legacy) |

> ⚠️ Do **not** copy the entire `AppData` folder — this can carry over the corruption.

### Step 3 – Restore Outlook profile (if needed)
1. Open **Control Panel > Mail > Show Profiles**
2. Add a new profile and configure the user's Exchange/Microsoft 365 account
3. Set it as the default profile

---

## 🛠️ Fix 3: Run System File Checker

Corruption can sometimes be caused by damaged system files.

```cmd
sfc /scannow
```

If issues are found and not repaired:
```cmd
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow
```

Reboot and test login again.

---

## 🛠️ Fix 4: Roaming Profile Issues (Domain Environment)

If the user has a roaming profile that is corrupted on the server:

1. Log in as admin on the affected machine
2. Rename the roaming profile folder on the server:
   - e.g. `\\server\profiles\username` → `\\server\profiles\username.old`
3. Have the user log in — Windows will create a fresh roaming profile
4. Manually copy data from the `.old` folder to the new profile

**Force a local profile temporarily**:
- In ADUC, open the user's properties > **Profile** tab
- Clear the **Profile path** field, apply, and have the user log in locally
- Restore the path once the profile is rebuilt

---

## 🛠️ Fix 5: Reset Profile via PowerShell

List all local profiles:
```powershell
Get-CimInstance -ClassName Win32_UserProfile | Select-Object LocalPath, Loaded, Special
```

Remove a corrupted profile (this deletes the profile folder — back up data first):
```powershell
Get-CimInstance -ClassName Win32_UserProfile | Where-Object { $_.LocalPath -like "*username*" } | Remove-CimInstance
```

The user will get a fresh profile on next login.

---

## ✅ Post-Fix Checklist

- [ ] User can log in with their normal profile (not a temporary one)
- [ ] Desktop, Documents, and pinned apps are restored
- [ ] Outlook connects and email is accessible
- [ ] Browser bookmarks/favourites are restored
- [ ] User confirms all important data is accessible
- [ ] Old profile folder archived or deleted after confirmation

---

## 🛠️ Troubleshooting

| Symptom | Check |
|---|---|
| Still loading temp profile after registry fix | Recheck registry SID keys, ensure `.bak` was fully renamed |
| Profile loads but data is missing | Data may be in `C:\Users\username.000` or `.bak` folder |
| Roaming profile keeps corrupting | Check network share permissions and disk quota |
| Error: profile service failed | Review Event Viewer > Windows Logs > Application for UserProfile errors |

---

*Environment: Windows 11, local and domain-joined machines*
