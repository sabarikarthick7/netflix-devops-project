<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/08/Netflix_2015_logo.svg/400px-Netflix_2015_logo.svg.png" alt="Netflix" width="200"/>

  ## DevOps Deployment Project
</div>

A Netflix-style streaming UI deployed with Docker, Kubernetes & Jenkins CI/CD.

![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-18-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white)

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Nginx |
| Backend | Node.js, Express |
| Containers | Docker (multi-stage builds) |
| Orchestration | Kubernetes (Minikube) |
| CI/CD | Jenkins |
| Registry | DockerHub |

---

## 🏗 Architecture

```
Developer
    │
    ▼
GitHub Repository
    │
    ▼
Jenkins CI/CD Pipeline
    ├── Install Dependencies
    ├── Run Tests
    ├── Build React App
    ├── Build Docker Images
    └── Push Images to DockerHub
    │
    ▼
DockerHub Registry
    │
    ▼
Kubernetes Cluster (Minikube)
    ├── Frontend Deployment (React + Nginx)
    ├── Backend Deployment (Node.js + Express)
    ├── Frontend Service (NodePort :30000)
    └── Backend Service (ClusterIP :5000)
    │
    ▼
User Access → http://<MINIKUBE-IP>:30000
```

---

## 📁 Project Structure

```
netflix-devops-project/
├── frontend/          # React app + Nginx + Dockerfile
├── backend/           # Express API + Dockerfile
├── docker/            # docker-compose.yml
├── kubernetes/        # K8s manifests (namespace, deployments, services)
├── jenkins/           # Jenkinsfile + setup script
└── README.md
```

---

## 🚀 Quick Start

### Option 1 — Docker Compose (Local)

```bash
git clone https://github.com/sabarikarthick7/netflix-devops-project.git
cd netflix-devops-project/docker
docker-compose up --build -d
```

- Frontend → `http://localhost:3000`
- Backend → `http://localhost:5000/api/movies`

### Option 2 — Kubernetes (Minikube)

```bash
# Start cluster
minikube start --driver=docker

# Deploy
kubectl apply -f kubernetes/namespace.yaml
kubectl apply -f kubernetes/backend-deployment.yaml
kubectl apply -f kubernetes/backend-service.yaml
kubectl apply -f kubernetes/frontend-deployment.yaml
kubectl apply -f kubernetes/frontend-service.yaml

# Get URL
minikube ip   # open http://<IP>:30000
```

---

## 🔁 Jenkins Pipeline

| Stage | Description |
|-------|-------------|
| Clone | Pull from GitHub |
| Install | `npm install` — frontend & backend (parallel) |
| Test | `npm test` — frontend & backend (parallel) |
| Build | React production build |
| Docker Login | Authenticate to DockerHub |
| Docker Build | Build both images (parallel) |
| Docker Push | Push both images (parallel) |

**Credentials required:** Add a `dockerhub` credential (username + password) in Jenkins global credentials.

**NodeJS tool required:** Add a NodeJS installation named `node18` in Manage Jenkins → Tools.

---

## 🌐 API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /health` | Health check |
| `GET /api/movies` | All categories |
| `GET /api/movies/featured` | Banner movie |
| `GET /api/movies/:category` | By category |
| `GET /api/movie/:id` | Single movie |

---

## ⚙️ Jenkins Server Setup

```bash
chmod +x jenkins/setup-jenkins.sh
sudo ./jenkins/setup-jenkins.sh
```

Installs: Java 17, Jenkins, Docker, kubectl, Node.js 18, Minikube.

---

## 👤 Author

**Sabarikarthick** · [GitHub](https://github.com/sabarikarthick7)
