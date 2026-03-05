# 💻 Windows 11 – Common Issues & Fixes

A quick-reference guide for the most frequent Windows 11 support tickets.

---

## 1. Computer Won't Start / Black Screen

- [ ] Hold power button 10 sec to force shutdown, then restart
- [ ] Check if monitor is connected and powered on
- [ ] Try `Win + Ctrl + Shift + B` to restart graphics driver
- [ ] Boot into Safe Mode: hold Shift while clicking Restart → Troubleshoot → Advanced → Startup Settings

---

## 2. Slow Performance

- [ ] Check Task Manager (`Ctrl + Shift + Esc`) – CPU, RAM, Disk usage
- [ ] Disable startup programs: Task Manager → Startup apps
- [ ] Run Disk Cleanup: search "Disk Cleanup" in Start
- [ ] Check for pending Windows Updates
- [ ] Restart if uptime is more than a few days

---

## 3. Windows Update Stuck or Failing

```
# Run in PowerShell as Administrator
Stop-Service wuauserv
Stop-Service bits
Stop-Service cryptsvc

Rename-Item C:\Windows\SoftwareDistribution SoftwareDistribution.old

Start-Service wuauserv
Start-Service bits
Start-Service cryptsvc
```
Then retry Windows Update.

---

## 4. Application Crashes or Won't Open

- [ ] Run the app as Administrator (right-click → Run as administrator)
- [ ] Check Event Viewer for error codes: `Win + R` → `eventvwr`
- [ ] Repair the application: Settings → Apps → find app → Modify/Repair
- [ ] Uninstall and reinstall

---

## 5. Sound Not Working

- [ ] Right-click speaker icon → Troubleshoot sound problems
- [ ] Check volume mixer – the app might be muted individually
- [ ] Check default output device: Settings → System → Sound
- [ ] Update or reinstall audio drivers via Device Manager

---

## 6. Printer Not Found (see also [printer-troubleshooting.md](./printer-troubleshooting.md))

- [ ] Restart Print Spooler: `services.msc` → Print Spooler → Restart
- [ ] Remove and re-add the printer
- [ ] Check if printer is shared correctly on the network

---

## 7. File Explorer Freezes or Crashes

```
# Restart File Explorer via Task Manager
Ctrl + Shift + Esc → Find "Windows Explorer" → Restart
```

Or run in Command Prompt:
```
taskkill /f /im explorer.exe
start explorer.exe
```

---

## Useful Shortcuts

| Shortcut | Action |
|----------|--------|
| `Win + X` | Quick admin menu |
| `Win + I` | Settings |
| `Ctrl + Shift + Esc` | Task Manager |
| `Win + R` | Run dialog |
| `Win + Pause` | System info |
| `msconfig` | System configuration |
| `eventvwr` | Event Viewer |
| `devmgmt.msc` | Device Manager |

---

## Notes

- Always check Event Viewer for specific error codes before escalating.
- Document the error message and steps taken in the ServiceNow ticket.
