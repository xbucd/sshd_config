# 🛡️ Hardened SSH Server Configuration 
## sshd_config

2 versions - minimal and comprehensive hardened SSH server configuration focused on key-based authentication and reduced attack surface.

> The minimal version is ideal for personal or lightweight servers; the comprehensive one extends it with extra hardening and logging options.

---

## ⚡ Features
- Root and password login disabled
- Only Ed25519 keys allowed
- Verbose logging with limited sessions
- No tunneling, X11, or TCP forwarding
- IPv4 only by default

---

## 🚀 Deployment 

1. Backup current config:
   ```bash
   sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

2. Clone or download

3. Copy desired config to `/etc/ssh/sshd_config.d`:
   ```bash
   sudo cp 99-security.conf /etc/ssh/sshd_config.d/
   
5. Enable and check service status:
   ```bash
   sudo systemctl enable sshd
   sudo systemctl status sshd

6. Reload service:
   ```bash
   sudo systemctl reload sshd

7. Verify connection in a new terminal

---

## 💡 Tips
- You can comment out or delete lines you don’t need.
- Or start with the minimal config and use the comprehensive one as a template for updates.

---
## 🧠 Monitoring & Forensic Testing
### 🪧 Banner /etc/ssh/BannerName

Purpose: Displays a legal or informational message before login (even before the username prompt).
Useful for:

- Legal login warnings (“Unauthorized access prohibited”)
- Honeypot scenarios — observe attacker reactions
- Forensic labs — simulate real-world login banners

**How to enable:**
1. Create the banner file:
```bash
sudo nano /etc/ssh/BannerName
```
Example content:
```markdown
************************************************************
* WARNING: Authorized access only!                         *
* All actions are monitored and logged for security review. *
************************************************************
```
2. Uncomment `Banner /etc/ssh/BannerName`
3. Reload the SSH service
   
### 🧾 SyslogFacility AUTHPRIV
Purpose: Sends SSH authentication and session logs to a restricted logging channel accessible only by privileged users.

- Debian/Ubuntu → /var/log/auth.log
- RHEL/CentOS → /var/log/secure

**How to enable:** 
1. Uncomment `SyslogFacility AUTHPRIV` and `LogLevel VERBOSE`
2. To monitor logs in real time:
```bash
sudo tail -f /var/log/auth.log
```
or
```bash
sudo journalctl -u sshd -f
```

> These settings are excellent for incident response, honeypot monitoring, and forensic analysis.

---

License: GNU GPL v3

Author: xbucd (2025)

- Created with guidance from public references and AI tools
- You are free to modify and redistribute it.
