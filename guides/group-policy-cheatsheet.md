# Group Policy Cheat Sheet

A quick reference for common Group Policy tasks in a Windows Server Active Directory environment.

---

## 🔧 Common GPO Commands (CMD / PowerShell)

| Task | Command |
|---|---|
| Force GP update | `gpupdate /force` |
| Force update + logoff | `gpupdate /force /logoff` |
| View applied policies | `gpresult /r` |
| Full GP report (HTML) | `gpresult /h C:\gpreport.html` |
| Check GP on remote PC | `gpresult /s COMPUTERNAME /r` |

---

## 📁 Common Policy Locations (Group Policy Management Editor)

### Password Policy
`Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy`

| Setting | Recommended Value |
|---|---|
| Minimum password length | 12 characters |
| Password complexity | Enabled |
| Maximum password age | 90 days |
| Enforce password history | 10 passwords |

---

### Account Lockout Policy
`Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy`

| Setting | Recommended Value |
|---|---|
| Account lockout threshold | 5 invalid attempts |
| Account lockout duration | 15 minutes |
| Reset lockout counter after | 15 minutes |

---

### Drive Mapping
`User Configuration > Preferences > Windows Settings > Drive Maps`

- Right-click > New > Mapped Drive
- Set drive letter, path (e.g. `\\server\share`), and Action: **Create** or **Replace**
- Use **Item-level targeting** to apply to specific users/groups

---

### Disable USB Storage
`Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access`

- Set **All Removable Storage classes: Deny all access** → Enabled

---

### Desktop Wallpaper
`User Configuration > Policies > Administrative Templates > Desktop > Desktop`

- Enable **Desktop Wallpaper**, set path to UNC path (e.g. `\\server\wallpapers\bg.jpg`)

---

### Software Restriction / AppLocker
`Computer Configuration > Policies > Windows Settings > Security Settings > Application Control Policies > AppLocker`

- Create rules for Executable, Windows Installer, Script, and Packaged App rules
- Use **Audit only** mode first before enforcing

---

### Logon Scripts
`User Configuration > Policies > Windows Settings > Scripts > Logon`

- Add a `.bat` or `.ps1` script to run at user logon
- Store scripts in `\\domain\SYSVOL\domain\Policies\{GPO-GUID}\User\Scripts\Logon\`

---

## 🔗 GPO Linking & Scope

| Task | Location |
|---|---|
| Link GPO to OU | Right-click OU in GPMC > Link an Existing GPO |
| Block inheritance | Right-click OU > Block Inheritance |
| Enforce GPO | Right-click GPO link > Enforced |
| Filter by security group | GPO > Scope tab > Security Filtering |

---

## 🛠️ Troubleshooting GPOs

1. Run `gpupdate /force` on the affected machine
2. Run `gpresult /h C:\gpreport.html` and open in browser
3. Check **Applied GPOs** vs **Denied GPOs** sections
4. Verify the computer/user is in the correct OU
5. Check Security Filtering — the object must have **Read** and **Apply Group Policy** permissions
6. Check WMI filters if used
7. Review **Event Viewer > Applications and Services Logs > Microsoft > Windows > GroupPolicy**

---

*Environment: Windows Server 2019/2022, Windows 11 clients*
