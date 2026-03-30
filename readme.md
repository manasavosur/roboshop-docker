# 🚀 RoboShop Microservices - Docker Compose Setup

## 📦 Prerequisites

Make sure you have installed:

* Docker
* Docker Compose

Verify:

```bash
docker -v
docker compose version
```

---

## 🏗️ Build & Run the Application

### 🔹 First Time Setup (Build all services)

```bash
docker compose up -d --build
```

* Builds all images from Dockerfiles
* Starts all containers in detached mode

---

### 🔹 Start Existing Containers (No Code Changes)

```bash
docker compose up -d
```

* Uses already built images
* Faster startup

---

### 🔹 Rebuild After Code Changes

```bash
docker compose up -d --build
```

Use this when:

* You modify code
* You update dependencies
* You change Dockerfiles

---

### 🔹 Rebuild Only One Service

```bash
docker compose up -d --build cart
```

Example:

* If only `cart` service code changed

---

## 🛑 Stop the Application

```bash
docker compose down
```

* Stops and removes containers
* Keeps images intact

---

## 🔄 Restart Services

```bash
docker compose restart
```

---

## 📊 Check Running Containers

```bash
docker ps
```

---

## 📜 View Logs

### All services:

```bash
docker compose logs
```

### Specific service:

```bash
docker compose logs cart
```

---

## 🌐 Access the Application

* Frontend: http://<EC2-IP>
* Cart API: http://<EC2-IP>:8080

---

## ⚠️ Important Notes

* Always use `--build` after making code changes
* Do not run multiple services inside one container
* Ensure all services are connected via the `roboshop` network
* Use service names (e.g., `cart`, `catalogue`) for internal communication

---

## 🧹 Clean Up (Optional)

Remove everything including volumes:

```bash
docker compose down -v
```

---

## 💡 Tip

If something breaks:

```bash
docker compose logs <service-name>
```

Example:

```bash
docker compose logs cart
```

---

## ✅ Architecture Overview

```
                ┌──────────────┐
                │   Browser    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │  Frontend    │
                └──────┬───────┘
                       │
 ┌──────────────┬──────┼──────────────┬──────────────┐
 ▼              ▼      ▼              ▼              ▼
User         Cart   Catalogue     Shipping        Payment
 │              │        │             │              │
 ▼              ▼        ▼             ▼              ▼
Redis         Redis   MongoDB        MySQL        RabbitMQ
 │                        │                           │
 ▼                        ▼                           ▼
MongoDB               (Product DB)                (Async Queue)
```
