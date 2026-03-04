# 🌐 Network Troubleshooting Guide

**Use case:** User cannot connect to the internet, network drives, or internal systems.

---

## Quick Diagnosis Checklist

Before diving in, ask the user:
- [ ] Is it only this device, or multiple devices?
- [ ] Did it work before? When did it stop?
- [ ] Wired or wireless connection?
- [ ] Any recent changes (Windows update, moved desk, new equipment)?

---

## Step 1 – Check Physical Connection

**Wired:**
- Is the Ethernet cable plugged in firmly at both ends?
- Is there a link light on the NIC and switch port?
- Try a different cable or port

**Wireless:**
- Is Wi-Fi enabled? (Check the hardware switch or `Fn` key)
- Is the correct network selected?
- Is the device too far from the access point?

---

## Step 2 – Run Basic Network Commands

Open Command Prompt (`Win + R` → `cmd`):

```
# Check IP configuration
ipconfig /all

# Release and renew IP address (if DHCP)
ipconfig /release
ipconfig /renew

# Flush DNS cache
ipconfig /flushdns

# Ping the default gateway (replace with actual gateway IP)
ping 192.168.1.1

# Ping external DNS to test internet
ping 8.8.8.8

# Ping by hostname to test DNS resolution
ping google.com

# Trace route to identify where connection drops
tracert google.com

# DNS lookup
nslookup google.com
```

---

## Step 3 – Interpret Results

| Symptom | Likely Cause | Action |
|---------|-------------|--------|
| No IP address (169.254.x.x) | DHCP failure | Check DHCP server, try `ipconfig /renew` |
| Can ping gateway but not internet | Router/firewall issue | Escalate to network admin |
| Can ping 8.8.8.8 but not google.com | DNS issue | Set DNS to 8.8.8.8 manually or flush DNS |
| Ping times out completely | Cable/switch issue | Check physical connection |
| Only one device affected | Device-specific issue | Restart NIC, check adapter settings |

---

## Step 4 – Reset Network Stack (Windows 11)

Run as Administrator:

```
netsh int ip reset
netsh winsock reset
ipconfig /flushdns
```

Restart the computer after running these commands.

---

## Step 5 – Escalate if Needed

Escalate to network admin if:
- Multiple users/devices are affected
- The issue is on the switch or router level
- VPN or firewall configuration changes are needed

---

## Notes

- Always document what commands were run and their output in the ServiceNow ticket.
- Take note of the IP, subnet mask, gateway, and DNS found with `ipconfig /all` – this is useful for escalation.
