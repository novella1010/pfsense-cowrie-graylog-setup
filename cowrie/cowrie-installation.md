# 🪤 Cowrie Honeypot Installation Guide (Raspberry Pi)

This section covers the setup and configuration of **Cowrie**, a medium-interaction SSH/Telnet honeypot, running on a **Raspberry Pi** inside a segmented VLAN.  
The goal is to safely attract and log unauthorized login attempts, command execution, and attacker behavior.

---

## ⚙️ 1. Reference Guide

Follow the official Cowrie installation steps from the 7-step guide:  
🔗 [Cowrie Installation Documentation](https://cowrie.readthedocs.io/en/latest/INSTALL.html)
🔗 [Cowrie SSH Logs](https://github.com/novella1010/pfsense-cowrie-graylog-setup/blob/main/graylog%20cowrie%20logs.png)

---

## 🧱 2. Setup Overview

- **Device:** Raspberry Pi 4  
- **OS:** Debian-based (Raspberry Pi OS / Ubuntu Server)  
- **Network:** Isolated VLAN (`HONEYPOT-VLAN`) called `HONEYPOT`  
- **Firewall:** pfSense (bare metal) controlling VLAN segmentation  
- **Log Forwarding:** Cowrie → Graylog (via GELF HTTP input)

---

## 🔐 3. SSH Configuration Changes

To prevent interference between your honeypot and your own SSH access:
- The real SSH daemon (`sshd`) was modified in `/etc/ssh/sshd_config`
- The listening port was changed from **22** to a **random non-standard port**
- Cowrie listens on **port 22** to attract attackers, while the real SSH daemon remains accessible on your custom port.

> Example:
> ```bash
> sudo nano /etc/ssh/sshd_config
> # Change:
> Port 22
> # To:
> Port 58291
> sudo systemctl restart ssh
> ```

This ensures **you retain secure administrative access** while allowing Cowrie to simulate a legitimate SSH endpoint.

---

## 📡 4. Graylog GELF HTTP Integration

Cowrie logs are forwarded to **Graylog** using the **GELF HTTP output plugin**.  
This enables structured JSON log ingestion, easy search, and dashboard visualization.

🔗 Reference: [Cowrie GELF HTTP Output Configuration](https://cowrie.readthedocs.io/en/latest/output_gelf.html)

Example from your `cowrie.cfg`:
```ini
[output_gelf]
enabled = true
gelf_url = http://graylog-containerIP:12201/gelf
