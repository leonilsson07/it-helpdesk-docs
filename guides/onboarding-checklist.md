# ✅ New User IT Onboarding Checklist

**Use case:** When a new employee starts, IT must complete the following tasks before or on their first day.

---

## 1. Before Start Date

- [ ] Verify that the user account is already present in Active Directory (accounts are pre-provisioned and included automatically during PXE installation)
- [ ] Assign Microsoft 365 license (via M365 Admin Center)
- [ ] Set temporary password and require change on first login
- [ ] Prepare and configure computer (Windows 11)
- [ ] Install computer via PXE (network boot) – domain join and AD account sync handled automatically
- [ ] Install role-specific software (varies per user – e.g. VPN client, warehouse systems, internal business applications)
- [ ] Configure Outlook with company email
- [ ] Set up MFA (Microsoft Authenticator)
- [ ] Confirm network and shared drive access
- [ ] Test printer access if relevant
- [ ] Verify Outlook, Teams, and OneDrive are working
---

## 2. On First Day

- [ ] Hand over computer and login credentials
- [ ] Walk user through first login and password change

---

## 3. After First Week

- [ ] Follow up – any issues or questions?
- [ ] Confirm all software is installed and working
- [ ] Close onboarding ticket in ServiceNow

---

## Notes

- Always document the ticket in ServiceNow with what was done and when.
- If user has a specific role (e.g. warehouse, office), adjust software and access accordingly.
- For VPN access, follow the separate VPN setup guide.
