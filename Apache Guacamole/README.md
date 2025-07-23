# 🖥️ Apache Guacamole Deployment with Docker Compose, MySQL, and NGINX

This repository provides a complete Docker Compose setup for deploying [Apache Guacamole](https://guacamole.apache.org/) — a powerful, clientless remote desktop gateway that supports RDP, VNC, and SSH — accessible directly through your web browser.

---

## 🚀 Overview

Apache Guacamole allows users to securely access remote machines from any modern browser, without needing additional plugins or client-side software. This setup uses:

- ✅ **MySQL** as a persistent backend database
- ✅ **guacd**, the Guacamole proxy daemon
- ✅ **Guacamole Web App** for frontend & API
- ✅ **NGINX** for reverse proxy, WebSocket support, and CORS handling

---

## 📐 Architecture Diagram

```
Browser ───> NGINX ───> Guacamole Web ───> guacd ───> [RDP / VNC / SSH Servers]
                          │
                          └────> MySQL Database
```

Each component is containerized, and they communicate over an isolated Docker network (`guacnet`).

---

## 🧱 Stack Components

### 1. MySQL
Stores all Guacamole configuration such as users, connections, history, etc.

### 2. guacd
The Guacamole daemon that handles all protocol-level remote desktop connections.

### 3. Guacamole Server
The web-based interface and API gateway that interacts with guacd and MySQL.

### 4. NGINX Reverse Proxy
Handles HTTP routing, WebSocket tunneling, and Cross-Origin Resource Sharing (CORS) headers.

---

## 🛠 Configuration Files

### 📄 `docker-compose.yml`

This file defines all services:

- `mysql` with persistent volume
- `guacd` with default settings
- `guacamole` connected to MySQL and guacd
- `nginx` configured to route WebSocket and HTTP traffic

### 📄 `nginx.conf`

Custom NGINX settings:

- WebSocket tunneling at `/guacamole/websocket-tunnel`
- CORS headers for front-end testing or cross-domain API calls
- Handles `OPTIONS` requests for preflight checks

---

## ⚙️ Environment Variables

Before running the stack, create a `.env` file in the root directory:

```env
MYSQL_VERSION=8.0
MYSQL_ROOT_PASSWORD=yourpassword
MYSQL_DATABASE=guacamole_db
MYSQL_USER=guac_user
MYSQL_PASSWORD=guac_pass
GUACAMOLE_VERSION=1.5.3
```

This keeps your credentials and versions customizable.

---

## 📦 How to Deploy

### Step 1: Clone the Repository

```bash
git clone https://github.com/your-org/guacamole-docker-setup.git
cd guacamole-docker-setup
```

### Step 2: Create the `.env` File

Refer to the section above and fill in your environment-specific values.

### Step 3: Start the Stack

```bash
docker-compose up -d
```

### Step 4: Access Guacamole

Open your browser and go to:

```
http://localhost:8080/guacamole
```

---

## 🧪 Test Login

After setup, you can log in using the default admin credentials (if initialized via SQL scripts) or configure users through the Guacamole web UI or database.

---

## 🧠 Tips for Production

- ✅ Use HTTPS: Wrap NGINX with Let's Encrypt or a reverse proxy like Traefik.
- ✅ Use secure secrets management for environment variables.
- ✅ Backup the MySQL volume (`guac-db-data`) regularly.
- ✅ Scale `guacamole` containers behind a load balancer for HA.

---

## 📁 Directory Structure

```
.
├── docker-compose.yml         # Core container definitions
├── nginx.conf                 # NGINX reverse proxy and WebSocket support
├── .env                       # Environment configuration (user-provided)
└── README.md                  # Project documentation
```

---

## 📖 References

- 🔗 Apache Guacamole Docs: https://guacamole.apache.org/doc/
- 🔗 Docker Hub (Guacamole): https://hub.docker.com/r/guacamole/guacamole
- 🔗 Docker Hub (guacd): https://hub.docker.com/r/guacamole/guacd

---

## 📄 License

This project is open-sourced under the **MIT License**.

---

## 🤝 Contributions

Contributions, bug reports, and suggestions are welcome! Submit a pull request or open an issue to get started.
