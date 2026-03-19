# Containerization-web-app
If your README.md is not updating on GitHub, it usually means the changes were saved locally on your computer, but they haven't been "pushed" (uploaded) to the GitHub servers yet.
Also, looking at the text you pasted, some of the Markdown formatting for your tables got a little messed up. Let's fix the formatting and push it up properly!
Step 1: Fix and Save your README.md
Open your README.md file in your code editor (like VS Code or Notepad). Replace everything in it with the corrected code below, which fixes the tables and code blocks so they look perfect on GitHub.
Make sure you hit "Save" (Ctrl+S) after pasting this!

Markdown


# Containerized Web Application with PostgreSQL using Docker Compose and Macvlan

# Containerized Web Application
### PostgreSQL with Docker Compose & Macvlan Networking

##  Overview
[cite_start]This project demonstrates a production-ready **containerized backend** architecture[cite: 11]. [cite_start]It features a Node.js (Express) REST API and a PostgreSQL database [cite: 12][cite_start], orchestrated via **Docker Compose** and connected through a high-performance **Macvlan network**.

Key highlights of this implementation include:
* [cite_start]**Layer 2 Isolation:** Containers are assigned unique MAC addresses and static IPs (192.168.10.200/201) on the physical LAN[cite: 41, 42, 51].
* [cite_start]**Extreme Optimization:** Utilization of **Multi-stage builds** and **Alpine Linux** to reduce image sizes by up to 86%[cite: 16, 21, 36].
* [cite_start]**Security:** Implementation of `.dockerignore` and non-root execution (`USER node`) to mitigate privilege escalation[cite: 23, 24].

---

## Architecture
```text
[ Physical Router / Gateway: 192.168.10.1 ]
              │
              ▼
    ┌──────────────────────┐
    │    custom_macvlan    │
    └──────────┬───────────┘
              │
      ┌───────┴───────┐
      │               │
  [Backend API]   [PostgreSQL]
  192.168.10.201  192.168.10.200

# Architecture

```text
Client (Throwaway Alpine Container on LAN)
      │
      ▼
Backend API Container (Node.js + Express)
192.168.10.201
      │
      ▼
PostgreSQL Container
192.168.10.200

________________________________________Project Structure

Plaintext


containerized-web-app
│
├── backend
│   ├── Dockerfile
│   ├── server.js
│   ├── package.json
│   └── .dockerignore
│
├── database
│   └── Dockerfile
│
├── docker-compose.yml
└── README.md

________________________________________Technology Stack
●	Backend: Node.js + Express
●	Database: PostgreSQL 15
●	Containerization: Docker
●	Orchestration: Docker Compose
●	Networking: Macvlan
________________________________________Docker Image Optimization
The project demonstrates Docker optimization techniques:
Optimized Build
●	Alpine base images (node:18-alpine, postgres:15-alpine)
●	Multi-stage builds (for the backend)
●	Non-root user (USER node)
●	Minimal layers & .dockerignore utilized
Non-Optimized Build
●	Standard images (e.g., node:18, postgres:15)
●	Single stage builds
●	Larger image sizes containing unnecessary OS utilities
________________________________________Create Network (Required)
Create the Macvlan network manually (bypassing compose) for the physical host attachment:

Bash


docker network create -d macvlan \
  --subnet=192.168.10.0/24 \
  --gateway=192.168.10.1 \
  -o parent=eth0 \
  custom_macvlan

Verify:

Bash


docker network inspect custom_macvlan

________________________________________Build and Run the Application
Build Containers

Bash


docker-compose up -d --build

Check Running Containers

Bash


docker ps

________________________________________Container IP Addresses
●	PostgreSQL: 192.168.10.200
●	Backend: 192.168.10.201
________________________________________API Endpoints
Health Check
GET /health

Bash


docker run --rm --network custom_macvlan alpine/curl [http://192.168.10.201:3000/health](http://192.168.10.201:3000/health)

Response:

JSON


{
  "status": "healthy",
  "timestamp": "2026-03-16T..."
}

Insert Record
POST /records

Bash


docker run --rm --network custom_macvlan alpine/curl -X POST [http://192.168.10.201:3000/records](http://192.168.10.201:3000/records) -H "Content-Type: application/json" -d '{"data":"Testing volume persistence!"}'

Fetch Records

JSON


[
 {
  "id": 1,
  "data": "Testing volume persistence!",
  "created_at": "2026-03-16T..."
 }
]

________________________________________Volume Persistence Test
Check named volume creation:

Bash


docker volume ls

Insert data using the POST command above. Stop and destroy containers:

Bash


docker-compose down

Restart containers:

Bash


docker-compose up -d

________________________________________Image Size Comparison
Image Type	Base Image Used	Approximate Standard Size	Optimized Size	Size Reduction
Backend API	node:18-alpine (Multi-stage)	~1.1 GB (node:18)	~150 MB	~86%
Database	postgres:15-alpine	~400 MB (postgres:15)	~250 MB	~37%
________________________________________Macvlan vs Ipvlan Comparison
Feature	Macvlan	Ipvlan
MAC Address	Unique per container	Shared with Host
Host communication	Blocked by default	Allowed
Isolation	True Layer 2 identity	L2 or L3 endpoints
Best Use Case	LAN device simulation	Strict switch port rules
________________________________________Screenshots Appendix
![Build Logs](images/build command.png)
![Build Step 2](images/build command 2.png)
![Docker container](images/Docker container.png)
________________________________________Author
Rishu kumar
B.Tech CSE (ccvt)
500126596



### Step 2: Push the Changes to GitHub
Now that the file is saved locally, you need to tell Git to upload the new changes. 

Open your WSL terminal, make sure you are in your `containerized-web-app` folder, and run these three commands exactly like you did before:

**1. Stage the new file and images:**
```bash
git add .

2. Commit the changes:

Bash


git commit -m "Update README with formatted tables and screenshots"

3. Push to GitHub:

Bash


git push

Once that git push command finishes successfully, go refresh your GitHub page in your browser. The beautifully formatted README should be right there on the front page! Let me know if it shows up.
