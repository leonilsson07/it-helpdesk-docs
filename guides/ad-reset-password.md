# 🔑 Active Directory – Reset User Password

**Use case:** User has forgotten their password or been locked out of their account.

---

## Prerequisites

- Access to Active Directory Users and Computers (ADUC)
- Admin or Helpdesk-level AD permissions

---

## Steps

### 1. Open Active Directory Users and Computers
- Press `Win + R`, type `dsa.msc`, press Enter
- Or search for "Active Directory Users and Computers" in Start

### 2. Find the User
- Right-click your domain → **Find**
- Search by name or username
- Double-click the user to open their properties

### 3. Reset the Password
- Right-click the user → **Reset Password**
- Enter a temporary password (follow company password policy)
- Check **"User must change password at next logon"**
- Click **OK**

### 4. Unlock Account (if locked)
- In the user's properties → **Account** tab
- Check if **"Unlock account"** checkbox is visible and check it
- Click **Apply** → **OK**

### 5. Communicate to User
- Send the temporary password via a secure method (phone call preferred, never plain email)
- Confirm the user can log in successfully

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Reset Password" is greyed out | You don't have sufficient permissions |
| User still can't log in after reset | Check if account is disabled (Account tab) |
| Password doesn't meet requirements | Check domain password policy in Group Policy |

---

## Notes

- Always log the ticket in ServiceNow with: time, username (not password), and action taken.
- Never share passwords over email or chat.
- If the issue persists, escalate to senior admin.
