# Containerized Web Application with PostgreSQL using Docker Compose and MACvlan


## Overview

This project demonstrates a containerized backend architecture using:

* Node.js (Express) REST API
* PostgreSQL Database
* Docker & Docker Compose
* Macvlan Networking (static IP-based communication)

The system simulates a real-world deployment where containers behave like physical devices on a local network.

---

## Architecture

```
Client (Browser / Postman)
        │
        ▼
Backend API (Node.js)
IP: 192.168.10.201
        │
        ▼
PostgreSQL Database
IP: 192.168.10.200
        │
        ▼
Docker Volume (Persistent Storage)
```

---

## Project Structure

```
containerized-web-app/
│
├── backend/
│   ├── Dockerfile
│   ├── server.js
│   ├── package.json
│   └── .dockerignore
│
├── database/
│   └── Dockerfile
│
├── docker-compose.yml
└── README.md
```

---

## Tech Stack

* **Backend:** Node.js + Express
* **Database:** PostgreSQL
* **Containerization:** Docker
* **Orchestration:** Docker Compose
* **Networking:** Macvlan

---

## Networking (Macvlan)

* Each container gets a unique IP address
* Containers appear as physical devices on the LAN
* No port mapping required
* High performance and network isolation

---

## Docker Image Optimization

This project follows best practices for optimized container builds:

* Alpine-based images (`node:18-alpine`, `postgres:15-alpine`)
* Multi-stage builds for smaller image size
* `.dockerignore` to exclude unnecessary files
* Non-root user execution for better security

---

## Create Network (Required)

```bash
docker network create -d macvlan \
  --subnet=192.168.10.0/24 \
  --gateway=192.168.10.1 \
  -o parent=eth0 \
  custom_macvlan
```

Verify:

```bash
docker network inspect custom_macvlan
```

---

## Run the Project

### Build and Start Containers

```bash
docker-compose up -d --build
```

### Check Running Containers

```bash
docker ps
```

---

## Container IP Addresses

* **Backend API:** 192.168.10.201
* **PostgreSQL:** 192.168.10.200

---



## Volume Persistence Test

1. Insert data using POST request
2. Stop containers:

```bash
docker-compose down
```

3. Restart containers:

```bash
docker-compose up -d
```

4. Fetch data again → Data remains intact

---

## Docker Volume Verification

```bash
docker volume ls
```

---

## Image Size Comparison

| Component   | Standard Image | Optimized Image | Reduction |
| ----------- | -------------- | --------------- | --------- |
| Backend API | ~1.1 GB        | ~150 MB         | ~86%      |
| PostgreSQL  | ~400 MB        | ~250 MB         | ~37%      |

---

## Macvlan vs IPvlan

| Feature            | Macvlan               | IPvlan             |
| ------------------ | --------------------- | ------------------ |
| MAC Address        | Unique per container  | Shared with host   |
| Host Communication | Blocked by default    | Allowed            |
| Isolation          | Full Layer 2 identity | L2 / L3 modes      |
| Use Case           | LAN simulation        | Cloud environments |

---

## Key Learnings

* Docker containerization
* Docker Compose orchestration
* Macvlan networking
* Persistent storage using volumes
* Backend–database communication
* DevOps best practices

## Conclusion

This project demonstrates a complete containerized backend system using Docker and Macvlan networking.

It ensures:

* Scalability
* High-performance networking
* Data persistence
* Production-ready architecture

This implementation reflects real-world DevOps practices and modern deployment strategies.

---

## Author

**Rishu Kumar**
B.Tech CSE (CCVT)
