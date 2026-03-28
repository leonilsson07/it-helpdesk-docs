# BitLocker Issues – Troubleshooting Guide

A step-by-step guide for diagnosing and resolving common BitLocker problems on Windows 11.

---

## 🔍 Check BitLocker Status

```powershell
manage-bde -status
```
Or in PowerShell:
```powershell
Get-BitLockerVolume
```

Shows encryption status, protection status, and key protectors for each drive.

---

## 🛠️ Common Issues & Fixes

### 1. BitLocker Recovery Key Prompt at Boot

**Cause**: Hardware change, BIOS/UEFI update, TPM issue, or boot order changed.

**Fix**:
1. Enter the **48-digit recovery key**
   - Find it in: **Azure AD / Microsoft Account > devices**, or **Active Directory > BitLocker Recovery**
   - Or check if saved to a USB, printed, or stored in a file
2. After logging in, suspend and resume BitLocker to reset the TPM seal:
```cmd
manage-bde -protectors -disable C:
manage-bde -protectors -enable C:
```
3. If triggered by a BIOS update, this is expected — enter the key once and it will not prompt again

**Retrieve recovery key from AD (PowerShell)**:
```powershell
Get-ADObject -Filter {objectClass -eq 'msFVE-RecoveryInformation'} -Properties msFVE-RecoveryPassword | Select-Object Name, msFVE-RecoveryPassword
```

---

### 2. BitLocker Won't Enable – TPM Not Found or Not Ready

**Cause**: TPM is disabled in BIOS, not initialized, or not present.

**Fix**:
1. Enter BIOS/UEFI > find **TPM** or **Security Chip** setting > enable it
2. Initialize TPM in Windows:
   - Open **tpm.msc** > click **Prepare the TPM**
3. Check TPM status in PowerShell:
```powershell
Get-Tpm
```
4. If no TPM is available, BitLocker can still be enabled with a USB startup key — requires Group Policy change:
   - `Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > Operating System Drives`
   - Enable **Require additional authentication at startup** and allow BitLocker without a compatible TPM

---

### 3. BitLocker Stuck Encrypting or Decrypting

**Cause**: Process paused, system sleep interrupted encryption, or disk error.

**Fix**:
1. Check current status:
```cmd
manage-bde -status C:
```
2. Resume encryption if paused:
```cmd
manage-bde -resume C:
```
3. If stuck for a long time, check for disk errors:
```cmd
chkdsk C: /f /r
```
(Requires reboot)
4. Reboot and check if encryption resumes automatically

---

### 4. Lost Recovery Key

**Cause**: Key was not backed up or the backup location is unknown.

**Where to look**:
- **Azure AD**: portal.azure.com > Devices > select device > BitLocker keys
- **Active Directory**: ADUC > Computer object > BitLocker Recovery tab (requires AD schema extension)
- **Microsoft Account**: account.microsoft.com/devices/recoverykey
- Local backup (if configured): `manage-bde -protectors -get C:`

**If the key is truly lost**:
- The drive cannot be decrypted without the key
- Data on the drive is unrecoverable
- The drive must be wiped and reimaged

> ⚠️ Always back up BitLocker recovery keys to AD or Azure AD during setup.

---

### 5. BitLocker Cannot Be Managed (Access Denied)

**Cause**: Group Policy restricts BitLocker management to admins only, or the user is not a local admin.

**Fix**:
1. Run Command Prompt or PowerShell as administrator
2. Check if GPO is controlling BitLocker:
   - `gpresult /h C:\gpreport.html` > look under BitLocker policies
3. Use MBAM (Microsoft BitLocker Administration and Monitoring) if deployed in the environment

---

### 6. BitLocker Not Compliant in Intune / Azure AD

**Cause**: Encryption report shows device as not encrypted despite BitLocker being active.

**Fix**:
1. Check encryption status:
```powershell
Get-BitLockerVolume | Select-Object MountPoint, VolumeStatus, EncryptionPercentage, KeyProtector
```
2. Ensure the recovery key is escrowed to Azure AD:
```powershell
BackupToAAD-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId (Get-BitLockerVolume -MountPoint "C:").KeyProtector[1].KeyProtectorId
```
3. Force a device sync in the Company Portal app or Intune portal
4. Review Intune device encryption report for specific compliance errors

---

## 🔑 Useful Commands Reference

| Task | Command |
|---|---|
| Check status | `manage-bde -status` |
| Enable BitLocker | `manage-bde -on C: -recoverypassword -skiphardwaretest` |
| Disable BitLocker | `manage-bde -off C:` |
| Suspend BitLocker | `manage-bde -protectors -disable C:` |
| Resume BitLocker | `manage-bde -protectors -enable C:` |
| Get recovery key | `manage-bde -protectors -get C:` |
| Backup key to AD | `manage-bde -protectors -adbackup C: -id {KeyProtectorID}` |
| Resume encryption | `manage-bde -resume C:` |

---

## ✅ Escalation Checklist

If the issue is not resolved:

- [ ] Note the exact error message or manage-bde output
- [ ] Confirm whether recovery key exists anywhere
- [ ] Check Event Viewer: **Applications and Services Logs > Microsoft > Windows > BitLocker-API**
- [ ] Escalate to L2/L3 with device name, error details, and encryption status output

---

*Environment: Windows 11, TPM 2.0, domain-joined and Azure AD-joined machines*
