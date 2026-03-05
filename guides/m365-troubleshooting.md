# 📧 Microsoft 365 – Common Issues & Fixes

Quick-reference guide for Outlook, Teams, OneDrive, and Office apps.

---

## Outlook

### Can't Send or Receive Email
- [ ] Check internet connection
- [ ] Look for "Disconnected" or "Working Offline" in the status bar → Click **Send/Receive** tab → Uncheck **Work Offline**
- [ ] Remove and re-add the account: File → Account Settings → Remove → Add
- [ ] Clear Outlook cache: close Outlook → delete contents of `%localappdata%\Microsoft\Outlook`

### Outlook Won't Open
- Run Outlook in safe mode: `Win + R` → `outlook.exe /safe`
- Disable add-ins: File → Options → Add-ins → Manage COM Add-ins → Disable all
- Repair Office: Settings → Apps → Microsoft 365 → Modify → Quick Repair

### Calendar Not Syncing
- [ ] Check if delegate/shared calendar permissions are correct in AD/M365
- [ ] Remove and re-add shared calendar
- [ ] Run: `outlook.exe /cleanfreebusy`

---

## Microsoft Teams

### Teams Not Starting
- Quit Teams completely (check system tray)
- Clear cache: `%appdata%\Microsoft\Teams` → delete all contents
- Restart Teams

### Camera or Mic Not Working in Meetings
- [ ] Check Windows privacy settings: Settings → Privacy & Security → Camera / Microphone
- [ ] In Teams: Settings (gear icon) → Devices → Select correct camera/mic
- [ ] Test in another app (e.g. Camera app) to rule out hardware issue

### Status Stuck on "Away" or "Offline"
- Sign out and sign back in
- Clear Teams cache (see above)
- Check if the user's M365 license includes Teams

---

## OneDrive

### Files Not Syncing
- [ ] Check OneDrive status icon in system tray (pause/resume sync)
- [ ] Sign out and back in: right-click OneDrive icon → Settings → Account → Unlink this PC → Sign in again
- [ ] Check storage quota in OneDrive settings

### "Files On-Demand" Issues
- Files may need to be downloaded: right-click file → Always keep on this device

---

## Office Apps (Word, Excel, PowerPoint)

### "Product Activation Failed"
- Sign out and back in: File → Account → Sign out → Sign in with company account
- Check M365 license assignment in Admin Center
- Run activation troubleshooter: `cscript "%programfiles%\Microsoft Office\Office16\OSPP.VBS" /act`

### App Crashing on Open
- Repair Office: Settings → Apps → Microsoft 365 → Modify → Online Repair

---

## Admin Tasks (M365 Admin Center)

| Task | Location |
|------|----------|
| Assign/remove license | Users → Active users → Select user → Licenses |
| Reset MFA | Users → Active users → Select user → Manage multifactor authentication |
| Reset password | Users → Active users → Select user → Reset password |
| Check mailbox size | Exchange Admin Center → Mailboxes |

---

## Notes

- Most M365 issues are solved by clearing cache or re-signing in.
- Always verify the user has the correct license assigned before troubleshooting further.
- Document error messages and steps in ServiceNow.
