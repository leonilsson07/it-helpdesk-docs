# 🖨️ Printer Troubleshooting Guide

**Use case:** User cannot print, printer is offline, or print jobs are stuck.

---

## Quick Checklist

- [ ] Is the printer powered on?
- [ ] Is it connected (USB or network)?
- [ ] Is there paper and toner/ink?
- [ ] Is the correct printer selected when printing?

---

## Step 1 – Restart Print Spooler

The Print Spooler is a common cause of printer issues in Windows.

```
# Open Services
Win + R → services.msc

# Find "Print Spooler" → Right-click → Stop

# Clear the print queue manually
Navigate to: C:\Windows\System32\spool\PRINTERS
Delete all files in this folder (not the folder itself)

# Go back to Services → Print Spooler → Start
```

Or via Command Prompt (Administrator):
```
net stop spooler
del /Q /F /S "%systemroot%\System32\spool\PRINTERS\*.*"
net start spooler
```

---

## Step 2 – Check Printer Status

- Open **Settings** → **Bluetooth & devices** → **Printers & scanners**
- Click the printer → **Open print queue**
- Cancel all stuck jobs
- Check if printer shows as **Offline**

**To fix "Offline" status:**
- Right-click printer → **See what's printing**
- Click **Printer** in the menu bar → Uncheck **Use Printer Offline**

---

## Step 3 – Network Printer Not Found

- Ping the printer IP to confirm it's reachable:
  ```
  ping 192.168.1.x
  ```
- Try adding the printer manually: Settings → Printers & scanners → **Add a printer** → **The printer I want isn't listed** → Add by IP
- Verify the printer is on the same VLAN/subnet as the user's computer

---

## Step 4 – Reinstall Printer Driver

- Settings → Printers & scanners → Select printer → **Remove**
- Download latest driver from manufacturer's website
- Re-add the printer

---

## Step 5 – Shared Network Printer (via Print Server)

- Check if the print server is online
- Re-add the printer via UNC path: `\\printserver\printername`
- Verify the user has permission to use the shared printer

---

## Common Error Messages

| Error | Solution |
|-------|----------|
| "Driver is unavailable" | Reinstall or update printer driver |
| "Access denied" | Check share permissions or user group membership |
| "Print spooler not running" | Start Print Spooler in services.msc |
| Jobs stuck in queue | Clear spooler (see Step 1) |

---

## Notes

- For network printers, always check physical connectivity (cable, switch port) before blaming software.
- Document the printer name/IP and steps taken in the ServiceNow ticket.
