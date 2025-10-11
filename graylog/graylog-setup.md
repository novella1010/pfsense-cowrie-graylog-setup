# 🧩 Graylog Configuration & Container Setup

This section documents the **Graylog** setup for centralized log management and visualization.  
Graylog aggregates logs from pfSense and Cowrie over **Syslog** and **GELF HTTP**, allowing event correlation and analysis.

---

## ⚙️ 1. Reference Documentation

- 🔗 [Cowrie GELF HTTP Output Setup](https://docs.cowrie.org/en/latest/graylog/README.html)
- 🔗 [Graylog Official Documentation](https://go2docs.graylog.org/current/downloading_and_installing_graylog/docker_installation.htm?tocpath=Install%20Graylog%7CContainerized%20Deployment%7C_____1)
- 🔗 [Graylog Log Screenshot](https://github.com/novella1010/pfsense-cowrie-graylog-setup/blob/main/graylog%20cowrie%20logs.png)
- 🔗 [Graylog Log Output](https://github.com/novella1010/pfsense-cowrie-graylog-setup/blob/main/graylog%20cowrie%20logs.png)
---

## 🐳 2. Docker Compose Setup

Below is a **redacted version** of the `docker-compose.yaml` used to deploy Graylog, MongoDB, and Datanode.  
All sensitive information (passwords, tokens, file paths) has been removed for security.

```yaml
version: "3.8"

services:
  mongodb:
    image: mongo:6.0
    volumes:
      - ./mongodb_data:/data/db
    restart: on-failure
    networks:
      - custom-bridge

  datanode:
    image: graylog/graylog-datanode:6.1
    environment:
      GRAYLOG_DATANODE_NODE_ID_FILE: /var/lib/graylog-datanode/node-id
      GRAYLOG_DATANODE_PASSWORD_SECRET: <redacted>
      GRAYLOG_DATANODE_ROOT_PASSWORD_SHA2: <redacted>
      GRAYLOG_DATANODE_MONGODB_URI: mongodb://mongodb:27017/graylog
      GRAYLOG_DATANODE_TLS_DISABLE: "true"
    ports:
      - 8999:8999
      - 9200:9200
      - 9300:9300
    networks:
      - custom-bridge
    volumes:
      - ./graylog-datanode:/var/lib/graylog-datanode
    restart: on-failure

  graylog:
    image: graylog/graylog:6.1
    environment:
      TZ: US/Eastern
      GRAYLOG_ROOT_TIMEZONE: US/Eastern
      GRAYLOG_NODE_ID_FILE: /usr/share/graylog/data/data/node-id
      GRAYLOG_PASSWORD_SECRET: <redacted>
      GRAYLOG_ROOT_PASSWORD_SHA2: <redacted>
      GRAYLOG_HTTP_BIND_ADDRESS: 0.0.0.0:9001
      GRAYLOG_HTTP_EXTERNAL_URI: http://localhost:9001/
      GRAYLOG_MONGODB_URI: mongodb://mongodb:27017/graylog
    ports:
      - 12201:12201/tcp
      - 12201:12201/udp
      - 9001:9001
    volumes:
      - ./graylog_data:/usr/share/graylog/data/data
      - ./graylog_journal:/usr/share/graylog/data/journal
    networks:
      - custom-bridge
    restart: on-failure

networks:
  custom-bridge:
    driver: bridge
