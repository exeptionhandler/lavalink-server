# 🎵 Lavalink Server (Enhanced Production Fork)

<div align="center">

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)
[![Docker](https://img.shields.io/badge/docker-ready-blue?style=for-the-badge&logo=docker)](https://hub.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)

**A production-focused Lavalink node with single-port architecture, integrated dashboard, and zero-friction deployment**

</div>

---

## 🚀 Overview

This project is an **enhanced fork** of a Lavalink server setup, redesigned with a strong focus on:

- real-world deployment constraints
- developer experience
- observability and monitoring

It is optimized specifically for platforms like **Render**, where traditional Lavalink setups fail due to multi-port limitations.

---

## 💡 Key Innovations

### 🔁 Single-Port Architecture
- Uses **Nginx reverse proxy** to serve:
  - Dashboard (`/`)
  - Lavalink API (`/v4/*`)
  - WebSocket (`/v4/websocket`)
- Solves **Render's single-port limitation**

---

### 📊 Integrated Dashboard
- Real-time monitoring (CPU, RAM, players, frames)
- Runs on the same port (no extra services)
- Mobile-friendly UI

---

### ⚡ Developer Experience
- One-click deployment
- Minimal configuration
- Pre-integrated plugins (LavaSrc + YouTube OAuth)

---

### 🔐 Secure by Design
- No hardcoded secrets
- Fully environment-based config
- OAuth handled safely via refresh tokens

---

## 🧠 Architecture

            ┌──────────────────────────┐
            │        Client/Bot        │
            └────────────┬─────────────┘
                         │
                         ▼
                ┌────────────────┐
                │   Nginx Proxy  │  (Port 2333)
                └───────┬────────┘
                        │
     ┌──────────────────┴──────────────────┐
     │                                     │

┌───────▼────────┐ ┌────────▼────────┐
│ Dashboard │ │ Lavalink │
│ (Static UI) │ │ (Java Node) │
│ "/" │ │ "/v4/*" │
└────────────────┘ └─────────────────┘


---

## 🧠 Architecture Decisions

### Why Nginx?
- Required for **port multiplexing**
- Handles HTTP + WebSocket cleanly
- Lightweight and production-proven

---

### Why single container?
- Simplifies deployment
- Avoids multi-service orchestration
- Ideal for PaaS (Render, Railway, etc.)

---

### Why dashboard inside same service?
- Eliminates need for external monitoring tools
- Faster debugging
- Better UX for developers

---

## 📁 Project Structure


.
├── nginx/ # Reverse proxy configuration
├── dashboard/ # Static frontend dashboard
├── application.yml # Lavalink configuration
├── Dockerfile # Optimized container setup
└── .env.example # Environment variables template


---

## 🔄 Differences from Upstream

| Feature                     | Original | This Fork |
|----------------------------|----------|----------|
| Single-port support        | ❌        | ✅        |
| Built-in dashboard         | ❌        | ✅        |
| Render-ready deployment    | ⚠️        | ✅        |
| Pre-configured plugins     | ❌        | ✅        |
| Simplified setup           | ❌        | ✅        |

---

## 🧪 Performance

Typical baseline:

- 🧠 RAM: 200MB – 500MB
- ⚙️ CPU: Low to moderate
- 🎧 Handles small–medium Discord bots comfortably

> Performance depends heavily on active players and filters used.

---

## 🚀 Quick Deploy

1. Click **Deploy to Render**
2. Set:

LAVALINK_SERVER_PASSWORD

3. Done ✅

---

## 🔐 Security

- Never commit `.env`
- Always use strong passwords
- Use **burner account** for YouTube OAuth
- Prefer HTTPS/WSS in production

---

## 🧠 Design Philosophy

This project prioritizes:

- **Practical deployment over complexity**
- **Developer experience over configuration overload**
- **Real-world constraints over theoretical setups**

---

## 📸 (Recommended)

> Add screenshots here to showcase the dashboard  
> This significantly improves perceived quality

---

## 📄 License

MIT
