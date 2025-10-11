pfSense + Cowrie + Graylog: Threat Detection & Honeypot Integration Lab

This project demonstrates how to integrate pfSense, Cowrie, and Graylog to simulate a real-world network security monitoring environment. It showcases how to securely deploy a honeypot, capture attacker behavior, and centralize logs for analysis and threat intelligence—all within a segmented network architecture.

🔍 Overview

This lab is built around a segmented homelab environment:

pfSense — running bare metal, acting as the primary firewall, router, and VLAN gateway. It segments the network into dedicated zones for management, servers, honeypots, and logging.

Cowrie Honeypot — deployed on a Raspberry Pi, isolated in a dedicated honeypot VLAN to attract and log unauthorized SSH/Telnet access attempts.

Graylog — running as a Docker container on a separate logging VLAN, aggregating logs from pfSense and Cowrie using Syslog and GELF inputs. It provides dashboards, alerting, and visualization for honeypot activity and network events.

🧩 Architecture Summary

Network Segmentation Example:

VLAN 10 → Management (pfSense & admin systems)

VLAN 20 → Application containers (including Graylog)

VLAN 50 → Honeypot zone (Raspberry Pi running Cowrie)

Each VLAN is isolated by pfSense firewall rules, ensuring that only controlled log traffic (e.g., Syslog over UDP/TCP) flows between zones. This setup mirrors how production networks implement DMZ-style segmentation for security monitoring and research.

⚙️ Features

Centralized log collection from pfSense and Cowrie in Graylog

Honeypot event monitoring, attacker session playback, and credential capture

Syslog forwarding and GELF input configuration for structured logging

Optional CrowdSec or IDS integration for extended threat intelligence

Prebuilt Graylog dashboards and extractors for quick setup

🎯 Purpose

This project was built as part of a personal cybersecurity lab to explore:

Safe deployment of a honeypot on a segmented network

Secure log forwarding and SIEM integration

Threat detection and correlation using open-source tools

Network monitoring and data visualization best practices

It’s ideal for students, blue teamers, and SOC analysts interested in practical network defense concepts.

🧰 Requirements

pfSense 2.7+ (bare metal or virtualized)

Raspberry Pi (4GB+ RAM) for Cowrie honeypot

Docker & Docker Compose for Graylog

Basic understanding of VLANs, Syslog, and firewall rules

📁 Repository Contents

pfsense-syslog-config.md → Setup for log forwarding to Graylog

cowrie-installation.md → Raspberry Pi deployment guide for Cowrie

graylog-setup.md → Docker Compose configuration and dashboards

⚠️ Disclaimer

This repository is for educational and research purposes only.
Do not expose your honeypot directly to the public internet without isolation and monitoring. The author assumes no liability for misuse or damage caused by the implementation of this lab.

Would you like me to add an ASCII-style network diagram or a Markdown diagram section to visually represent the pfSense–Cowrie–Graylog flow? That would make your README stand out to recruiters and other GitHub viewers.
