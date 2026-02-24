# DevOps Assignment – MEAN Stack CI/CD Deployment

## 📌 Project Overview

This project demonstrates the containerization and cloud deployment of a full-stack CRUD application built using the **MEAN stack**:

- MongoDB  
- Express.js  
- Angular 15  
- Node.js  

The application manages a collection of tutorials. Each tutorial contains:

- ID  
- Title  
- Description  
- Published Status  

Users can:

- Create tutorials  
- Retrieve tutorials  
- Update tutorials  
- Delete tutorials  
- Search tutorials by title  

---

# 🖥️ Local Development Setup (Original Project)

## 🔹 Backend Setup

```bash
cd backend
npm install
node server.js
```

MongoDB credentials can be updated in:

```
backend/app/config/db.config.js
```

---

## 🔹 Frontend Setup

```bash
cd frontend
npm install
ng serve --port 8081
```

Navigate to:

```
http://localhost:8081
```

Frontend API interaction logic is located in:

```
src/app/services/tutorial.service.ts
```

---

# 🐳 Dockerized Deployment

The application has been fully containerized using Docker and deployed on AWS EC2 using Docker Compose.

## 📦 Docker Architecture

The deployment includes the following services:

- **Nginx** – Reverse Proxy (Port 80 exposed)
- **Angular Frontend** – Internal port 8081
- **Node.js Backend** – Internal port 8080
- **MongoDB** – Official mongo:7 image with persistent volume

Only port **80** is exposed publicly via Nginx.

All services communicate internally using Docker networking.

---

# ☁️ Cloud Infrastructure

## EC2 Instances Used

### 1️⃣ Jenkins Server (CI/CD)
- Ubuntu 24.04 LTS
- Docker Installed
- Jenkins Installed
- Responsible for build and deployment

### 2️⃣ Application Server (Runtime)
- Ubuntu 24.04 LTS
- Docker & Docker Compose Installed
- Runs application containers

---

# 🏗️ Deployment Architecture

```
Developer → GitHub → Jenkins → Docker Hub → App EC2
                                            ↓
                                   Nginx → Angular
                                            ↓
                                           Node
                                            ↓
                                         MongoDB
```

---

# 🔄 CI/CD Pipeline

The CI/CD pipeline is implemented using **Jenkins**.

## Pipeline Stages

1. Checkout source code from GitHub  
2. Build Docker images (frontend & backend)  
3. Push images to Docker Hub  
4. SSH into Application EC2  
5. Pull latest images  
6. Restart containers using Docker Compose  

Deployment is fully automated.

Any push to GitHub triggers the pipeline via webhook.

---

# 🐳 Docker Hub Images

- `lokesh3721/angular-app`
- `lokesh3721/backend`

---

# 🚀 Manual Deployment (If Required)

On Application EC2:

```bash
docker compose pull
docker compose up -d
```

Application is accessible at:

```
http://13.233.108.247/tutorials
```

---

# 🔐 Security Configuration

- Only port 80 exposed publicly  
- Backend & MongoDB not exposed  
- Separate EC2 instances for Jenkins and Runtime  
- Docker internal networking used for service communication  

---

# 🧪 How to Reproduce the Setup

## Jenkins Server Setup

```bash
sudo apt update
sudo apt install docker.io
sudo usermod -aG docker jenkins
```

Install required Jenkins plugins:

- Git  
- GitHub  
- Docker Pipeline  
- SSH Agent  

Configure:

- Docker Hub credentials  
- EC2 SSH credentials  
- GitHub Webhook  

---

## Application Server Setup

```bash
sudo apt update
sudo apt install docker.io docker-compose
git clone https://github.com/lokicodess/devops-mean-app-deployment.git
cd devops-mean-app-deployment
docker compose up -d
```

---

# 📸 Screenshots

### 1️⃣ Jenkins Successful Pipeline Execution  
📌 Screenshot of completed pipeline with all stages green  
<img width="1439" height="432" alt="image" src="https://github.com/user-attachments/assets/dfb567d2-b7db-4ca4-827c-d3e7587a3620" />
<img width="1431" height="687" alt="image" src="https://github.com/user-attachments/assets/0a20467b-eaa0-4548-afd4-25d2ae6028f9" />
<img width="1440" height="684" alt="image" src="https://github.com/user-attachments/assets/254cd4b0-7365-4a9b-bdfb-0192e50844bc" />
<img width="1440" height="696" alt="image" src="https://github.com/user-attachments/assets/18582f52-0d6c-4d6e-a938-7489cedbc457" />
<img width="1439" height="684" alt="image" src="https://github.com/user-attachments/assets/44f60fff-f632-41b1-a253-9fba8e6bd99a" />
<img width="1440" height="686" alt="image" src="https://github.com/user-attachments/assets/5e50c64a-cb43-41b5-a6ac-5a3ba6199189" />
<img width="1437" height="614" alt="image" src="https://github.com/user-attachments/assets/3541dd13-c1d7-4184-b3cc-d7ea446e819b" />



### 2️⃣ Docker Hub Images  
📌 Screenshot showing angular-app and backend repositories  
<img width="1158" height="181" alt="image" src="https://github.com/user-attachments/assets/1195cf4c-2180-4516-986c-c0a756ffdc72" />


### 3️⃣ Running Application in Browser  
📌 Screenshot of working CRUD UI from EC2 public IP  
<img width="2048" height="1079" alt="image" src="https://github.com/user-attachments/assets/f7cab924-8cbf-4bcc-8651-b52403a4888b" />
<img width="2048" height="1078" alt="image" src="https://github.com/user-attachments/assets/686a4d8d-bc2b-4caf-aaf1-b357176b5cd0" />
<img width="2048" height="1076" alt="image" src="https://github.com/user-attachments/assets/369f3b40-3373-40bb-b093-39e713fccc66" />



### 4️⃣ Docker Containers Running  
📌 Screenshot of `docker ps` output on Application EC2  

<img width="2048" height="426" alt="image" src="https://github.com/user-attachments/assets/adb6ab5d-51a5-43e9-a00d-7b773c75f239" />


### 5️⃣ EC2 Instances Overview  
📌 Screenshot of AWS EC2 dashboard showing Jenkins and App instances  
<img width="2048" height="940" alt="image" src="https://github.com/user-attachments/assets/60b0aee1-560d-45a8-a63a-b93aae59026c" />

