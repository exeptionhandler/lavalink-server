# 🎵 Lavalink Server (Enhanced Production Fork)

<div align="center">

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)
[![Docker](https://img.shields.io/badge/docker-ready-blue?style=for-the-badge&logo=docker)](https://hub.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)

**A production-focused Lavalink node with single-port architecture, integrated dashboard, and zero-friction deployment**

</div>

---

## 🚀 Overview

This project is an enhanced fork of a Lavalink server setup, redesigned for:

- real-world deployment constraints
- better developer experience
- built-in observability

---

## 💡 Key Innovations

### 🔁 Single-Port Architecture
- Nginx reverse proxy
- Supports HTTP + WebSocket
- Works on Render (single-port limit)

### 📊 Integrated Dashboard
- Real-time metrics
- Same port as Lavalink
- No extra services

### ⚡ Developer Experience
- One-click deploy
- Minimal config
- Plugins ready out-of-the-box

---

## 🧠 Architecture

```
Client → Nginx (Port 2333)
        ├── /        → Dashboard
        └── /v4/*    → Lavalink
```

---

## 📁 Structure

```
.
├── nginx/
├── dashboard/
├── application.yml
├── Dockerfile
└── .env.example
```

---

## 🔄 Differences from upstream

- Dashboard added
- Single-port proxy
- Better deployment UX
- Pre-configured plugins

---

## 🧪 Performance

- RAM: ~200–500MB
- CPU: low–moderate

---

## 🚀 Deploy

1. Deploy to Render
2. Set password
3. Done

---

## 🔐 Security

- No hardcoded secrets
- Use env variables
- Use burner account for YouTube OAuth

---

## 🧠 Philosophy

Simple > complex
Real-world > theoretical

---

## 📄 License

MIT
