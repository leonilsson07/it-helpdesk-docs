# 👤 Active Directory – Create New User Account

**Use case:** A new employee needs an AD account set up before or on their start date.

---

## Prerequisites

- Access to Active Directory Users and Computers (ADUC)
- Admin or Helpdesk-level AD permissions
- New employee information (full name, department, role, start date)

---

## Steps

### 1. Open Active Directory Users and Computers
- Press `Win + R`, type `dsa.msc`, press Enter

### 2. Navigate to the Correct OU
- Expand the domain tree
- Go to the OU matching the user's department (e.g. `corp.local > Users > IT`)

### 3. Create the User
- Right-click the OU → **New** → **User**
- Fill in:
  - **First name / Last name**
  - **User logon name** (follow company naming convention, e.g. `firstname.lastname`)
- Click **Next**

### 4. Set Password
- Enter a temporary password following company password policy
- Check:
  - ✅ **User must change password at next logon**
  - ❌ User cannot change password *(leave unchecked)*
  - ❌ Password never expires *(leave unchecked unless special role)*
- Click **Next** → **Finish**

### 5. Configure the Account
- Right-click the new user → **Properties**
- **General tab:** Add email address, office, phone number
- **Account tab:** Verify logon name and set account expiry if temporary employee
- **Member Of tab:** Add to relevant security groups (e.g. `IT-Staff`, `VPN-Users`)

### 6. Verify
- Try logging in as the user
- Confirm access to shared drives, email, and required systems

---

## Naming Convention Example

| Full Name | Username | Email |
|-----------|----------|-------|
| Jane Doe | jane.doe | jane.doe@company.com |

---

## Notes

- Always document the new account in ServiceNow with the ticket number, creation date, and assigned OU/groups.
- Coordinate with manager to confirm correct group memberships.
- For M365 license assignment, go to the Microsoft 365 Admin Center after account creation.
