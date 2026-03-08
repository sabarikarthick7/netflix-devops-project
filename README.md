<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/0/08/Netflix_2015_logo.svg" width="200" alt="Netflix Logo"/>

# Netflix Clone — DevOps Deployment Project

**A production-grade Netflix UI clone deployed using Docker, Kubernetes & Jenkins CI/CD**

[![React](https://img.shields.io/badge/React-18.2.0-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.18.2-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![Docker](https://img.shields.io/badge/Docker-Multi--Stage-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Minikube-326CE5?style=flat-square&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=flat-square&logo=jenkins&logoColor=white)](https://jenkins.io/)
[![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-009639?style=flat-square&logo=nginx&logoColor=white)](https://nginx.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [UI Features](#-ui-features)
- [API Endpoints](#-api-endpoints)
- [Local Setup — Docker Compose](#-local-setup--docker-compose)
- [Kubernetes Deployment](#-kubernetes-deployment)
- [Jenkins CI/CD Pipeline](#-jenkins-cicd-pipeline)
- [Jenkins Setup Script](#-jenkins-setup-script)
- [Useful Commands](#-useful-commands)
- [Expected Output](#-expected-output)

---

## 🎯 Overview

This project is a **full-stack Netflix UI clone** built as a complete DevOps implementation. It demonstrates real-world practices including containerisation with Docker, orchestration with Kubernetes, and automated CI/CD with Jenkins — all running on Ubuntu.

Key highlights:
- **Zero API key dependency** — all movie data is served from an internal Express API using static JSON, so the UI always works
- **Fallback-safe frontend** — if the backend is unreachable, the React app loads built-in data automatically
- **Production Dockerfiles** — both services use multi-stage builds for minimal, secure images
- **Full CI/CD pipeline** — 8 Jenkins stages from clone to DockerHub push, with parallel execution where possible

---

## 🏗 Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                        CI/CD PIPELINE                            │
│                                                                  │
│  GitHub Repo ──► Jenkins (port 8080)                             │
│                      │                                           │
│          ┌───────────┼───────────────────────────┐               │
│          │     8-Stage Pipeline                  │               │
│          │  1. Clone Repository                  │               │
│          │  2. Install Dependencies (parallel)   │               │
│          │  3. Run Tests (parallel)              │               │
│          │  4. Build Frontend (React build)      │               │
│          │  5. Docker Login                      │               │
│          │  6. Build Docker Images (parallel)    │               │
│          │  7. Push to DockerHub (parallel)      │               │
│          │  8. Post: success / failure hooks     │               │
│          └───────────────────────────────────────┘               │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼ docker push
                    ┌─────────────────────┐
                    │      DockerHub      │
                    │  netflix-frontend   │
                    │  netflix-backend    │
                    └────────┬────────────┘
                             │ kubectl apply
                             ▼
┌────────────────────────────────────────────────────────────────────┐
│               KUBERNETES CLUSTER (Minikube)                        │
│                  Namespace: netflix-clone                          │
│                                                                    │
│  ┌─────────────────────────┐   ┌──────────────────────────────┐   │
│  │   Frontend Deployment   │   │    Backend Deployment        │   │
│  │      2 replicas         │   │       2 replicas             │   │
│  │                         │   │                              │   │
│  │  ┌───────────────────┐  │   │  ┌────────────────────────┐  │   │
│  │  │  Nginx + React    │  │   │  │  Node.js + Express     │  │   │
│  │  │  Port: 80         │  │   │  │  Port: 5000            │  │   │
│  │  └───────────────────┘  │   │  └────────────────────────┘  │   │
│  └────────────┬────────────┘   └──────────────┬───────────────┘   │
│               │                               │                    │
│  ┌────────────▼────────────┐   ┌──────────────▼───────────────┐   │
│  │  Frontend Service       │   │  Backend Service (ClusterIP) │   │
│  │  NodePort: 30000        │   │  Port: 5000                  │   │
│  └─────────────────────────┘   └──────────────────────────────┘   │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
                    │
                    ▼  http://<MINIKUBE-IP>:30000
              [ Browser / User ]
```

---

## 🛠 Tech Stack

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| **Frontend** | React.js | 18.2.0 | Netflix-style UI |
| **Frontend** | Axios | 1.6.2 | HTTP client |
| **Frontend** | Nginx | Alpine | Static file serving + proxy |
| **Backend** | Node.js | 18.x | Runtime |
| **Backend** | Express.js | 4.18.2 | REST API server |
| **Backend** | CORS | 2.8.5 | Cross-origin support |
| **Data** | Static JSON | — | No external API key needed |
| **Containerisation** | Docker | 24+ | Multi-stage image builds |
| **Orchestration** | Docker Compose | 3.8 | Local multi-container setup |
| **Orchestration** | Kubernetes | — | Production deployment |
| **Local K8s** | Minikube | Latest | Local cluster |
| **CI/CD** | Jenkins | LTS | 8-stage pipeline |
| **Registry** | DockerHub | — | Image storage |
| **OS** | Ubuntu | 22.04 LTS | Host environment |

---

## 📁 Project Structure

```
netflix-devops-project/
│
├── frontend/                          # React.js Application
│   ├── public/
│   │   └── index.html                 # HTML entry point
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.js / .css       # Sticky navbar with scroll state
│   │   │   ├── Banner.js / .css       # Full-viewport hero banner
│   │   │   ├── MovieRow.js / .css     # Horizontal scrollable row
│   │   │   ├── MovieCard.js / .css    # Card with hover popup
│   │   │   ├── Footer.js / .css       # Netflix-style footer
│   │   │   └── LoadingScreen.js / .css
│   │   ├── App.js                     # Root component + API fetch + fallback
│   │   ├── App.css                    # Global styles & CSS variables
│   │   └── index.js                   # React entry point
│   ├── Dockerfile                     # Multi-stage: Node build → Nginx serve
│   ├── nginx.conf                     # SPA routing, gzip, caching, proxy
│   ├── .dockerignore
│   └── package.json
│
├── backend/                           # Node.js Express API
│   ├── src/
│   │   ├── server.js                  # Express app with 6 endpoints
│   │   └── movies.js                  # Static movie data (6 categories)
│   ├── Dockerfile                     # Multi-stage: builder → non-root production
│   ├── .dockerignore
│   └── package.json
│
├── docker/
│   └── docker-compose.yml             # Local dev: health checks, networking
│
├── kubernetes/                        # Full production K8s manifests
│   ├── namespace.yaml
│   ├── backend-deployment.yaml        # 2 replicas, resource limits, probes
│   ├── backend-service.yaml           # ClusterIP on port 5000
│   ├── frontend-deployment.yaml       # 2 replicas, resource limits, probes
│   └── frontend-service.yaml          # NodePort 30000
│
├── k8s/                               # Simplified K8s manifests (quick deploy)
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   └── frontend-service.yaml
│
├── jenkins/
│   ├── Jenkinsfile                    # 8-stage declarative pipeline
│   └── setup-jenkins.sh               # One-command Ubuntu install script
│
├── .gitignore
└── README.md
```

---

## 🎨 UI Features

| Component | Features |
|-----------|----------|
| **Loading Screen** | Netflix-style red progress bar animation |
| **Navbar** | Fixed position, background darkens on scroll, search bar expand/collapse, profile dropdown, Netflix SVG logo |
| **Banner** | Full-viewport background, zoom-in animation, Play & More Info buttons, mute toggle, match %, genre tags |
| **Movie Rows** | 5 themed rows (Trending, Originals, Action, Comedies, Docs), horizontal scroll, left/right arrow buttons |
| **Movie Card** | Hover scale popup with match %, play/like/add-to-list buttons, genre label |
| **Footer** | Links grid, language selector, Netflix-style layout |
| **Responsive** | Mobile, tablet and desktop friendly |

---

## 🌐 API Endpoints

Base URL: `http://localhost:5000`

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Health check — returns status + timestamp |
| `GET` | `/api/movies` | All movie categories |
| `GET` | `/api/movies/featured` | Featured / banner movie |
| `GET` | `/api/movies/trending` | Trending now |
| `GET` | `/api/movies/netflixOriginals` | Netflix originals |
| `GET` | `/api/movies/actionAdventure` | Action & adventure |
| `GET` | `/api/movies/comedies` | Comedies |
| `GET` | `/api/movies/documentaries` | Documentaries |
| `GET` | `/api/movie/:id` | Single movie by ID |

---

## 🐳 Local Setup — Docker Compose

### Prerequisites

```bash
# Install Docker
sudo apt-get install -y docker.io
sudo systemctl start docker

# Install Docker Compose (if not bundled)
sudo apt-get install -y docker-compose
```

### Run the project

```bash
# 1. Clone the repository
git clone https://github.com/sabarikarthick7/netflix-devops-project.git
cd netflix-devops-project

# 2. Start all services
cd docker
docker-compose up --build -d

# 3. Check running containers
docker ps

# 4. Open in browser
#    Frontend → http://localhost:3000
#    Backend  → http://localhost:5000/api/movies
```

### Stop services

```bash
docker-compose down
```

### View logs

```bash
docker-compose logs -f frontend
docker-compose logs -f backend
```

---

## ☸️ Kubernetes Deployment

### Step 1 — Start Minikube

```bash
minikube start --driver=docker --memory=4096 --cpus=2
minikube status
```

### Step 2 — Build & Push Docker Images

```bash
# Set your DockerHub username
export DOCKER_USER=your_dockerhub_username

# Login
docker login

# Build and push backend
cd backend
docker build -t $DOCKER_USER/netflix-backend:latest .
docker push $DOCKER_USER/netflix-backend:latest
cd ..

# Build and push frontend
cd frontend
docker build \
  --build-arg REACT_APP_API_URL=http://netflix-backend-service:5000 \
  -t $DOCKER_USER/netflix-frontend:latest .
docker push $DOCKER_USER/netflix-frontend:latest
cd ..
```

### Step 3 — Update Image Names

```bash
# Replace placeholder with your DockerHub username in YAML files
sed -i "s/YOUR_DOCKERHUB_USERNAME/$DOCKER_USER/g" kubernetes/backend-deployment.yaml
sed -i "s/YOUR_DOCKERHUB_USERNAME/$DOCKER_USER/g" kubernetes/frontend-deployment.yaml
```

### Step 4 — Apply Manifests

```bash
# Create namespace
kubectl apply -f kubernetes/namespace.yaml

# Deploy backend
kubectl apply -f kubernetes/backend-deployment.yaml
kubectl apply -f kubernetes/backend-service.yaml

# Deploy frontend
kubectl apply -f kubernetes/frontend-deployment.yaml
kubectl apply -f kubernetes/frontend-service.yaml
```

### Step 5 — Verify

```bash
# Check pods
kubectl get pods -n netflix-clone

# Check services
kubectl get services -n netflix-clone

# Wait for rollout
kubectl rollout status deployment/netflix-backend  -n netflix-clone
kubectl rollout status deployment/netflix-frontend -n netflix-clone
```

### Step 6 — Access the App

```bash
# Get Minikube IP
minikube ip
# Example: 192.168.49.2

# Open: http://192.168.49.2:30000

# OR use Minikube's tunnel shortcut
minikube service netflix-frontend-service -n netflix-clone --url
```

---

## 🔁 Jenkins CI/CD Pipeline

### Pipeline Stages

```
Stage 1  → Clone Repository          git clone from GitHub (branch: main)
Stage 2  → Install Dependencies       npm install — frontend & backend (parallel)
Stage 3  → Run Tests                  npm test — frontend & backend (parallel)
Stage 4  → Build Frontend             npm run build (React production build)
Stage 5  → Docker Login               Authenticate with DockerHub credentials
Stage 6  → Build Docker Images        docker build — frontend & backend (parallel)
Stage 7  → Push Docker Images         docker push both images with BUILD_NUMBER tag (parallel)
Post     → Always / Success / Failure pipeline status hooks
```

### Required Jenkins Credentials

Go to **Manage Jenkins → Credentials → Global** and add:

| Credential Type | ID | Value |
|---|---|---|
| Username with password | `dockerhub` | DockerHub username + password |

> The Jenkinsfile reads this as `DOCKERHUB_CREDS` and auto-splits it into `DOCKERHUB_CREDS_USR` and `DOCKERHUB_CREDS_PSW`.

### Required Jenkins Plugin

- **NodeJS Plugin** — The pipeline uses `tools { nodejs 'node18' }`. Go to **Manage Jenkins → Tools → NodeJS** and add an installation named exactly `node18`.

### Create the Pipeline Job

1. **New Item → Pipeline**
2. Name: `netflix-clone-pipeline`
3. Pipeline Definition: **Pipeline script from SCM**
4. SCM: **Git**
5. Repository URL: `https://github.com/sabarikarthick7/netflix-devops-project.git`
6. Branch: `main`
7. Script Path: `jenkins/Jenkinsfile`
8. Click **Save → Build Now**

---

## ⚙️ Jenkins Setup Script

The `jenkins/setup-jenkins.sh` script installs everything needed on a fresh Ubuntu machine in one command:

```bash
chmod +x jenkins/setup-jenkins.sh
sudo ./jenkins/setup-jenkins.sh
```

**What it installs:**

| Step | Tool | Notes |
|------|------|-------|
| 1 | Java 17 (OpenJDK) | Required by Jenkins |
| 2 | Jenkins LTS | Runs on port 8080 |
| 3 | Docker | Adds `jenkins` user to docker group |
| 4 | kubectl | Latest stable release |
| 5 | Node.js 18 | Via NodeSource repository |
| 6 | Minikube | For local Kubernetes cluster |

After running, retrieve the Jenkins initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## 🔧 Useful Commands

### Docker

```bash
# Build images manually
docker build -t netflix-backend:latest  ./backend
docker build -t netflix-frontend:latest ./frontend

# Run backend standalone
docker run -p 5000:5000 netflix-backend:latest

# Run frontend standalone
docker run -p 3000:80 netflix-frontend:latest

# Check container logs
docker logs netflix-backend
docker logs netflix-frontend
```

### Kubernetes

```bash
# All resources in namespace
kubectl get all -n netflix-clone

# Describe a pod
kubectl describe pod <pod-name> -n netflix-clone

# Stream logs
kubectl logs -f deployment/netflix-backend  -n netflix-clone
kubectl logs -f deployment/netflix-frontend -n netflix-clone

# Shell into a pod
kubectl exec -it <pod-name> -n netflix-clone -- sh

# Delete everything
kubectl delete namespace netflix-clone

# Minikube dashboard
minikube dashboard
```

### Jenkins

```bash
# Service status
sudo systemctl status jenkins

# Restart Jenkins
sudo systemctl restart jenkins

# View Jenkins logs
sudo journalctl -u jenkins -f
```

---

## ✅ Expected Output

### Docker Compose

```
$ docker ps
CONTAINER ID   IMAGE                    STATUS          PORTS
a1b2c3d4e5f6   netflix-frontend:latest  Up (healthy)    0.0.0.0:3000->80/tcp
f6e5d4c3b2a1   netflix-backend:latest   Up (healthy)    0.0.0.0:5000->5000/tcp
```

### Kubernetes Pods

```
$ kubectl get pods -n netflix-clone
NAME                                 READY   STATUS    RESTARTS   AGE
netflix-backend-7d8f9c647b-xk2p9    1/1     Running   0          2m
netflix-backend-7d8f9c647b-mb4n7    1/1     Running   0          2m
netflix-frontend-5b6c8d94f-lp3q1    1/1     Running   0          90s
netflix-frontend-5b6c8d94f-wr9k2    1/1     Running   0          90s
```

### Kubernetes Services

```
$ kubectl get services -n netflix-clone
NAME                       TYPE        CLUSTER-IP      PORT(S)        AGE
netflix-backend-service    ClusterIP   10.96.45.12     5000/TCP       2m
netflix-frontend-service   NodePort    10.96.123.45    80:30000/TCP   90s
```

### Backend Health Check

```
$ curl http://localhost:5000/health
{
  "status": "ok",
  "message": "Netflix Clone API is running",
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

## 👤 Author

**Sabarikarthick**
- GitHub: [@sabarikarthick7](https://github.com/sabarikarthick7)

---

<div align="center">

Built with React · Node.js · Docker · Kubernetes · Jenkins

</div>
