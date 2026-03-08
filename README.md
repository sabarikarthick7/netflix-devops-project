# 🎬 Netflix Clone --- Full DevOps CI/CD Deployment

A production-style DevOps CI/CD pipeline deploying a Netflix-style web
application using Docker, Kubernetes, and Jenkins.

This project demonstrates how modern DevOps tools automate building,
containerizing, and deploying applications.

Frontend: React + Nginx\
Backend: Node.js + Express

------------------------------------------------------------------------

# 📐 Architecture

GitHub ↓ Jenkins CI/CD Pipeline │ ├─ Clone Repository ├─ Install
Dependencies ├─ Run Tests ├─ Build React Frontend ├─ Build Docker Images
├─ Push Images to DockerHub └─ Deploy to Kubernetes ↓ Kubernetes Cluster
│ ├─ Frontend Deployment (React + Nginx) ├─ Backend Deployment
(Node.js + Express) │ ├─ Frontend Service (NodePort) └─ Backend Service
(ClusterIP) ↓ Users Access Application

------------------------------------------------------------------------

# 🛠 Tech Stack

Frontend: React 18, CSS3, Nginx\
Backend: Node.js, Express\
Containerization: Docker\
Container Registry: DockerHub\
Orchestration: Kubernetes (Minikube)\
CI/CD: Jenkins\
Version Control: GitHub

------------------------------------------------------------------------

# 📁 Project Structure

Netflix-DevOps-Project │ ├── frontend/ │ ├── src/ │ ├── Dockerfile │ └──
nginx.conf │ ├── backend/ │ ├── src/ │ ├── Dockerfile │ └── package.json
│ ├── docker/ │ └── docker-compose.yml │ ├── kubernetes/ │ ├──
backend-deployment.yaml │ ├── backend-service.yaml │ ├──
frontend-deployment.yaml │ └── frontend-service.yaml │ ├── jenkins/ │
└── Jenkinsfile │ └── README.md

------------------------------------------------------------------------

# 🚀 Run Locally with Docker

Clone repository

git clone https://github.com/YOUR_USERNAME/Netflix-DevOps-Project.git\
cd Netflix-DevOps-Project

Start containers

docker-compose -f docker/docker-compose.yml up --build

Frontend: http://localhost:3000

Backend API: http://localhost:5000/api/movies

Stop containers

docker-compose down

------------------------------------------------------------------------

# ☸️ Deploy to Kubernetes

Start Minikube

minikube start

Deploy application

kubectl apply -f kubernetes/

Verify pods

kubectl get pods

Check services

kubectl get svc

Access application

minikube service netflix-frontend-service

------------------------------------------------------------------------

# ⚙️ Jenkins CI/CD Pipeline

Pipeline stages:

1.  Clone repository
2.  Install dependencies
3.  Run tests
4.  Build React frontend
5.  Docker login
6.  Build Docker images
7.  Push images to DockerHub
8.  Deploy to Kubernetes
9.  Verify deployment

------------------------------------------------------------------------

# 📌 DevOps Concepts Demonstrated

• CI/CD pipeline automation\
• Docker containerization\
• DockerHub image registry\
• Kubernetes deployments and services\
• Microservices architecture\
• Infrastructure as Code

------------------------------------------------------------------------

# 👨‍💻 Author

Sabari Karthick\
Aspiring DevOps Engineer

------------------------------------------------------------------------

# ⭐ Project Goal

To demonstrate a production-style DevOps pipeline integrating:

GitHub → Jenkins → Docker → DockerHub → Kubernetes
