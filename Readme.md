# 🚀 3-Tier Application Practical — VanRaj Royal Hotel Management System

## 🌟 Project Overview

This project demonstrates the implementation of a complete 3-Tier Application Architecture using modern cloud-native technologies.

The application is designed as a Hotel Management System where users can manage guest bookings, room allocation, booking status, and revenue tracking.

The project follows the industry-standard 3-tier architecture model:

* Presentation Tier (Frontend)
* Application Tier (Backend)
* Data Tier (Database)

Each tier is containerized using Docker and managed using Docker Compose.

### 🎯 Goal

Build and deploy a scalable, containerized, and cloud-ready hotel management system where all tiers communicate independently while maintaining high availability, modularity, and maintainability.

---

# 🏗️ 3-Tier Architecture Diagram

```text
                User Browser
                      ↓
        +---------------------------+
        |     Frontend Tier         |
        |  React + Nginx (3000)     |
        +-------------+-------------+
                      ↓
        +---------------------------+
        |      Backend Tier         |
        | Spring Boot API (8080)   |
        +-------------+-------------+
                      ↓
        +---------------------------+
        |       Database Tier       |
        | PostgreSQL DB (5432)      |
        +---------------------------+
```

This architecture separates:

* User Interface
* Business Logic
* Database Operations

which improves scalability, security, maintainability, and deployment flexibility.

---

# 🧰 Tech Stack

| Technology     | Purpose                    |
| -------------- | -------------------------- |
| React + Vite   | Frontend UI                |
| Nginx          | Reverse Proxy & Web Server |
| Spring Boot    | Backend REST API           |
| PostgreSQL     | Relational Database        |
| Docker         | Containerization           |
| Docker Compose | Multi-container Management |
| AWS EC2        | Cloud Hosting              |
| Linux          | Server Operating System    |
| Git & GitHub   | Version Control            |

---

# ⚙️ Project Structure

```text
student-app/
│
├── frontend/
│   ├── src/
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── nginx.conf
│   ├── Dockerfile
│   └── vite.config.js
│
├── backend/
│   ├── src/main/java/
│   │   ├── controller/
│   │   ├── service/
│   │   ├── repository/
│   │   ├── model/
│   │   └── config/
│   ├── application.properties
│   └── Dockerfile
│
├── init.sql
├── docker-compose.yml
└── README.md
```

---

# 🖥️ Tier 1 — Frontend Layer

## React + Nginx

The frontend layer is responsible for:

* User Interface
* Dashboard display
* Booking forms
* API communication
* Real-time data visualization

### Features

* Search bookings
* Add new bookings
* Update guest details
* Track room availability
* Revenue statistics
* Status filtering

### Frontend Port

```text
3000
```

---

# ⚙️ Tier 2 — Backend Layer

## Spring Boot REST API

The backend layer acts as the brain of the application.

Responsibilities:

* Business Logic
* REST API handling
* Validation
* Database operations
* CRUD operations
* Error handling

### API Endpoints

| Method | Endpoint                | Purpose              |
| ------ | ----------------------- | -------------------- |
| GET    | /api/guests             | Fetch bookings       |
| GET    | /api/guests/stats       | Dashboard statistics |
| POST   | /api/guests             | Create booking       |
| PUT    | /api/guests/{id}        | Update booking       |
| PATCH  | /api/guests/{id}/status | Update status        |
| DELETE | /api/guests/{id}        | Delete booking       |

### Backend Port

```text
8080
```

---

# 🗄️ Tier 3 — Database Layer

## PostgreSQL Database

The database layer stores all hotel-related information.

### Stored Data

* Guest details
* Room information
* Booking dates
* Booking status
* Revenue records
* Contact information

### Database Port

```text
5432
```

---

# 🐳 Docker Architecture

## Docker Compose Configuration

```yaml
services:

  db:
    image: postgres:16-alpine
    container_name: vanraj_db

  backend:
    build: ./backend
    container_name: vanraj-backend
    depends_on:
      db:
        condition: service_healthy

  frontend:
    build: ./frontend
    container_name: vanraj_frontend
    depends_on:
      backend:
        condition: service_healthy
```

---

# 🔁 Application Flow

## Step-by-Step Request Flow

### Step 1 — User Opens Frontend

```text
http://EC2_PUBLIC_IP:3000
```

The browser loads the React frontend application.

---

## Step 2 — Frontend Sends API Request

Frontend sends requests like:

```text
/api/guests
```

---

## Step 3 — Nginx Proxies Request

Nginx forwards API requests to the backend container.

```nginx
location /api/ {
    proxy_pass http://vanraj-backend:8080/api/;
}
```

---

## Step 4 — Backend Processes Request

Spring Boot:

* validates request
* executes business logic
* interacts with PostgreSQL
* generates API response

---

## Step 5 — Database Returns Data

PostgreSQL returns requested records to the backend.

---

## Step 6 — Frontend Displays Data

Frontend updates:

* dashboard
* booking table
* statistics
* revenue information

---

# ☁️ AWS EC2 Deployment

## Step 1 — Launch EC2 Instance

### Recommended Configuration

| Setting       | Value            |
| ------------- | ---------------- |
| AMI           | Ubuntu 24.04 LTS |
| Instance Type | t2.micro         |
| Storage       | 8GB              |
| Region        | ap-south-1       |

### Security Group Ports

| Port | Purpose     |
| ---- | ----------- |
| 22   | SSH         |
| 3000 | Frontend    |
| 8080 | Backend API |
| 5432 | PostgreSQL  |
| 80   | HTTP        |

---

# 🔐 Connect to EC2

```bash
ssh -i key.pem ubuntu@PUBLIC_IP
```

---

# 🧪 Install Docker

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
```

Add user to Docker group:

```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

Verify:

```bash
docker --version
docker-compose --version
```

---

# 📥 Clone Project

```bash
git clone https://github.com/YOUR_USERNAME/vanraj-hotel.git
```

Go into project folder:

```bash
cd vanraj-hotel/student-app
```

---

# 🚀 Run Application

## Build & Start Containers

```bash
docker-compose up -d --build
```

Wait 60 seconds for Spring Boot initialization.

---

# 🔍 Verify Containers

```bash
docker ps
```

Expected Output:

```text
vanraj_frontend    Up
vanraj-backend     Up
vanraj_db          Up
```

---

# 🧪 Testing Application

## Test Backend API

```bash
curl http://localhost:8080/api/guests
```

---

## Test Frontend

```bash
curl http://localhost:3000
```

---

## Open Application in Browser

```text
http://EC2_PUBLIC_IP:3000
```

---

# 🛠️ Important Configuration Files

## Frontend API Configuration

```javascript
const BASE = import.meta.env.VITE_API_BASE_URL || "/api";
```

---

## Nginx Reverse Proxy

```nginx
location /api/ {
    proxy_pass http://vanraj-backend:8080/api/;
}
```

---

## Database Connection

```properties
spring.datasource.url=jdbc:postgresql://db:5432/vanraj_hotel
```

---

# 🚨 Problems Faced & Solutions

| Problem              | Cause                      | Solution                  |
| -------------------- | -------------------------- | ------------------------- |
| Failed to fetch API  | Wrong backend hostname     | Changed to vanraj-backend |
| Tomcat host error    | Underscore not allowed     | Replaced _ with -         |
| Docker storage full  | Unused images & containers | docker system prune -af   |
| API routing failed   | Wrong nginx proxy          | Updated nginx.conf        |
| Frontend cache issue | Old JS bundle cached       | Hard reload browser       |
| CORS issue           | Missing EC2 IP             | Updated WebConfig.java    |

---

# 🎯 Key Learnings

* 3-Tier Architecture
* Docker Containerization
* Reverse Proxy using Nginx
* REST API Development
* PostgreSQL Integration
* AWS EC2 Deployment
* Docker Networking
* Linux Server Management
* Full Stack Application Deployment

---

# 💡 Real-World Use Cases

* Hotel Management Systems
* Banking Applications
* E-Commerce Platforms
* Enterprise Applications
* Healthcare Systems
* ERP Solutions

---

# 📸 Suggested Screenshots

* EC2 Dashboard
* Docker Containers
* Application UI
* API Response
* PostgreSQL Database
* Docker Compose Output
* Nginx Configuration

---

# ⭐ Final Result

✅ Successfully built a complete 3-Tier Application

✅ Frontend connected with backend using Nginx reverse proxy

✅ Spring Boot backend connected to PostgreSQL database

✅ All services containerized using Docker

✅ Managed multiple containers using Docker Compose

✅ Successfully deployed on AWS EC2

✅ Real-time hotel booking management system working successfully

---

# 🔥 Pro Tip

Always separate frontend, backend, and database into independent containers because it improves:

* scalability
* maintainability
* deployment flexibility
* fault isolation
* performance optimization
