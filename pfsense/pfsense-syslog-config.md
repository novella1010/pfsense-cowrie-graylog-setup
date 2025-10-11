#  pfSense Syslog & Network Segmentation Configuration

This section explains how pfSense (running bare metal) was configured to safely isolate the **Cowrie Honeypot** and forward logs to **Graylog**.

---

## ⚙️ 1. Network Architecture

- **pfSense (bare metal):** Core router/firewall
- **Raspberry Pi:** Cowrie honeypot in VLAN `HONEYPOT-VLAN`
- **Graylog Container:** Running on another VLAN `CONTAINER-VLAN`
- **Segmentation:** Ensures Cowrie cannot reach internal networks except Graylog via controlled rules

---

## 🔒 2. VLAN Rules & Port Forwarding

Below is a screenshot of the **pfSense firewall ruleset** for the honeypot VLAN:

![pfSense Honeypot Ruleset](<img width="1204" height="453" alt="image" src="https://github.com/user-attachments/assets/3e9596b4-e635-4c8a-aa4f-cc2880653439" />
)

### Explanation:
- The **HONEYPOT subnet** is restricted to **internet-only** traffic
- A **log-forwarding rule** allows traffic from the honeypot VLAN to Graylog (port `12201`)
- The **SSH honeypot port (2222)** is **NAT-forwarded** to Cowrie’s internal port **22**
  - This makes it appear as a standard SSH service to external attackers  
  - Example pfSense Port Forward:
    ```
    WAN TCP 22 → HONEYPOT-IP:2222
    ```
---

## 🧩 3. Syslog Forwarding

pfSense also forwards its own logs to **Graylog** via UDP/TCP Syslog input, allowing centralized monitoring of both:
- Firewall events (block/allow)
- Honeypot connection logs
- Network-based anomalies

---

## ✅ 4. Summary

- Bare-metal pfSense manages VLAN segmentation  
- Honeypot VLAN safely isolated from internal assets  
- Graylog receives both firewall and honeypot logs  
- SSH port-forwarding allows external simulation and testing
