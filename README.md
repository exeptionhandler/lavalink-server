# 🎵 Lavalink Server (Enhanced Fork)

<div align="center">

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)](LICENSE)

**Production-ready Lavalink node with single-port architecture, integrated dashboard, and simplified deployment**

</div>

---

## 🚀 What makes this fork different?

This is not just another Lavalink setup. This fork introduces:

- 🔁 **Single-port architecture** using Nginx (ideal for Render & similar platforms)
- 📊 **Integrated real-time dashboard** served on the same port
- ⚡ **Simplified deployment** (1-click + minimal configuration)
- 🔌 **Pre-configured plugins** (LavaSrc + YouTube OAuth)
- 🔐 **Secure environment-based configuration**

---

## 🧠 Architecture Overview


Client → Nginx (Port 2333)
├── / → Dashboard (static frontend)
└── /v4/* → Lavalink backend (API + WebSocket)


### Why this matters

- Platforms like Render only allow **one exposed port**
- Avoids running multiple services
- Clean separation between frontend and backend

---

## 📁 Project Structure


.
├── nginx/ # Reverse proxy configuration
├── dashboard/ # Static frontend dashboard
├── application.yml # Lavalink configuration
├── Dockerfile # Container setup
└── .env.example # Environment variables template


---

## 🔄 Differences from upstream

Compared to the original project, this fork:

- Adds a **built-in dashboard**
- Implements **single-port reverse proxy**
- Improves **deployment experience**
- Pre-configures plugins and OAuth flow

---

## 🧪 Performance Notes

Typical usage:

- 🧠 RAM: ~200MB–500MB
- ⚙️ CPU: Low–moderate (depends on active players)
- 🎧 Suitable for small to medium Discord bots

---

## 🚀 Quick Deploy

1. Click deploy (Render)
2. Set:
   - `LAVALINK_SERVER_PASSWORD`
3. Done ✅

---

## 🔐 Security Notes

- Never commit `.env` files
- Always use strong passwords
- Use a burner account for YouTube OAuth

---

## 🧠 Author Notes

This project focuses on **real-world deployment**, prioritizing:

- simplicity
- compatibility with cloud platforms
- minimal setup friction

---

## 📄 License

MIT
