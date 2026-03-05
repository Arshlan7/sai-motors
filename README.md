# 🔧 Sai Motors – Workshop Website

> **Automotive Maintenance & Repair Center** | Dholka, Ahmedabad

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)

---

## 📋 Table of Contents

- [About](#about)
- [Services](#services)
- [Project Structure](#project-structure)
- [🚀 Deploy on a Fresh Server](#-deploy-on-a-fresh-server-step-by-step)
- [CI/CD Pipeline](#cicd-pipeline)
- [Docker Hub](#docker-hub)
- [Contact](#contact)

---

## 🏢 About

**Sai Motors** is a full-service automotive workshop located in Dholka, Ahmedabad. This repository contains the official website — a static multi-page site built with HTML, CSS, and JavaScript, served via **Nginx** inside a **Docker** container.

---

## 🛠 Services

| Page | Description |
|------|-------------|
| `index.html` | Home page with hero slider & service overview |
| `mechanical-repair.html` | Engine, suspension, brakes, clutch & cooling |
| `periodic-maintenance.html` | Oil change, filters, inspections |
| `tyre-services.html` | Tyre replacement, balancing & alignment |
| `painting-denting.html` | Dent removal, body repair & painting |
| `battery-electrical.html` | Battery, alternator & wiring repairs |
| `ac-climate-care.html` | AC service, gas refill & cooling repair |

---

## 📁 Project Structure

```
sai-motors/
├── index.html
├── mechanical-repair.html
├── periodic-maintenance.html
├── tyre-services.html
├── painting-denting.html
├── battery-electrical.html
├── ac-climate-care.html
├── styles.css
├── car1.png / car2.png / car3.png
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── .dockerignore
├── .gitignore
└── .github/
    └── workflows/
        └── docker-publish.yml
```

---

## 🚀 Deploy on a Fresh Server (Step-by-Step)

> ✅ Tested on **Amazon Linux / RHEL / CentOS**
> These steps will take your website from zero to live in under 10 minutes.

---

### 📌 Prerequisites
- A fresh Linux server (AWS EC2, DigitalOcean, etc.)
- Port **8080** open in your firewall / security group
- Root or sudo access

---

### Step 1 — Update the Server

```bash
yum update -y
```

---

### Step 2 — Install Git

```bash
yum install git -y

# Verify
git --version
```

---

### Step 3 — Install Docker

```bash
yum install docker -y

# Start Docker service
systemctl start docker

# Enable Docker to start on reboot
systemctl enable docker

# Verify
docker --version
```

---

### Step 4 — Clone the Repository

```bash
git clone https://github.com/Arshlan7/sai-motors.git
cd sai-motors
```

---

### Step 5 — Build the Docker Image

```bash
docker build -t arshlan7/sai-motors:latest .
```

> ⏳ This will take 1–2 minutes on first run (downloads Nginx base image)

---

### Step 6 — Run the Container

```bash
docker run -d -p 8080:80 --name sai-motors --restart unless-stopped arshlan7/sai-motors:latest
```

| Flag | Meaning |
|------|---------|
| `-d` | Run in background |
| `-p 8080:80` | Map server port 8080 → container port 80 |
| `--name sai-motors` | Give container a friendly name |
| `--restart unless-stopped` | Auto-restart on server reboot |

---

### Step 7 — Verify Container is Running

```bash
docker ps
```

You should see:
```
CONTAINER ID   IMAGE                        STATUS         PORTS
xxxxxxxxxxxx   arshlan7/sai-motors:latest   Up X seconds   0.0.0.0:8080->80/tcp
```

---

### Step 8 — Test Locally on the Server

```bash
curl http://localhost:8080
```

> You should see HTML output of the website ✅

---

### Step 9 — Open Port 8080 in AWS Security Group

1. Go to **AWS Console → EC2 → Your Instance**
2. Click **Security Groups → Inbound Rules → Edit Inbound Rules**
3. Click **Add Rule** and set:
   - **Type:** Custom TCP
   - **Port range:** `8080`
   - **Source:** `0.0.0.0/0`
4. Click **Save Rules**

---

### Step 10 — Access the Website in Browser

First get your server's public IP:
```bash
curl ifconfig.me
```

Then open in your browser:
```
http://<your-server-public-ip>:8080
```

> 🎉 Your Sai Motors website is now LIVE!

---

### ⚡ Alternative — Pull Directly from Docker Hub (Fastest Method)

If you don't want to clone and build, just pull the pre-built image:

```bash
# Install Docker (Steps 1–3 above), then run:

docker pull arshlan7/sai-motors:latest

docker run -d -p 8080:80 --name sai-motors --restart unless-stopped arshlan7/sai-motors:latest
```

> No need to clone the repo or build anything! ✅

---

### 🔄 How to Update Website on Server

Whenever a new version is pushed to Docker Hub:

```bash
# Stop and remove old container
docker stop sai-motors
docker rm sai-motors

# Pull latest image
docker pull arshlan7/sai-motors:latest

# Run new container
docker run -d -p 8080:80 --name sai-motors --restart unless-stopped arshlan7/sai-motors:latest
```

---

### 🛑 Useful Docker Commands

```bash
# View running containers
docker ps

# View all containers (including stopped)
docker ps -a

# View container logs
docker logs sai-motors

# Stop the website
docker stop sai-motors

# Start the website again
docker start sai-motors

# Remove the container
docker rm sai-motors

# Remove the image
docker rmi arshlan7/sai-motors:latest
```

---

## 🔄 CI/CD Pipeline

Every push to the `main` branch automatically:

1. ✅ Checks out the latest code
2. ✅ Sets up Docker Buildx
3. ✅ Logs into Docker Hub
4. ✅ Builds the Docker image
5. ✅ Pushes `latest` + commit SHA tags to Docker Hub

### Required GitHub Secrets

Go to: `Repository → Settings → Secrets and variables → Actions → New repository secret`

| Secret Name | Value |
|-------------|-------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub Access Token **(Read & Write)** |

> Generate token at: **hub.docker.com → Account Settings → Security → New Access Token**

---

## 🐋 Docker Hub

Image is publicly available at:

```bash
docker pull arshlan7/sai-motors:latest
```

🔗 **https://hub.docker.com/r/arshlan7/sai-motors**

---

## 📞 Contact

| | |
|---|---|
| 📍 **Address** | Shop No. 07, Parshwanth Complex, Dholka, Ahmedabad |
| 📞 **Phone** | +91 98255 05076 |
| 💬 **WhatsApp** | [Chat Now](https://wa.me/919825505076) |

---

## 📄 License

© 2026 Sai Motors. All rights reserved.
