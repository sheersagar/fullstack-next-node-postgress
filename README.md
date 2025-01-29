🚀 Full-Stack Node.js Application

📝 Overview

This is a full-stack Node.js application with a separate frontend and backend. The application is containerized using Docker, built and deployed using Jenkins, and hosted on an EC2 instance. The Docker images are uploaded to Docker Hub via a CI/CD pipeline.

📂 Project Structure

├── frontend/       # React/Vue/Angular Frontend Code
│   ├── src/
│   ├── public/
│   ├── package.json
│   ├── Dockerfile
│   ├── .env
│   └── ...
├── backend/        # Node.js + Express Backend Code
│   ├── src/
│   ├── routes/
│   ├── models/
│   ├── package.json
│   ├── Dockerfile
│   ├── .env
│   └── ...
├── jenkinsfile     # Jenkins Pipeline Configuration
└── README.md       # Project Documentation

⚙️ Tech Stack

Frontend: React.js / Vue.js / Angular

Backend: Node.js, Express.js

Database: PostgreSQL / MongoDB (if applicable)

Containerization: Docker

CI/CD: Jenkins

Hosting: AWS EC2

🚀 Running the Application Locally

🔧 Prerequisites

Make sure you have the following installed:

Node.js

Docker

Jenkins

AWS EC2 Instance (if deploying to AWS)

🔹 Clone the Repository

git clone https://github.com/your-repo.git
cd your-repo

🔹 Running Backend

cd backend
npm install
npm start

🔹 Running Frontend

cd frontend
npm install
npm start

🐳 Docker Setup

🔹 Build and Run Frontend

cd frontend
docker build -t your-dockerhub-username/frontend-image .
docker run -d -p 3000:3000 your-dockerhub-username/frontend-image

🔹 Build and Run Backend

cd backend
docker build -t your-dockerhub-username/backend-image .
docker run -d -p 5000:5000 your-dockerhub-username/backend-image

🚀 CI/CD Pipeline with Jenkins

The Jenkinsfile automates the build, test, and deployment process:

pipeline {
    agent any
    
    environment {
        DOCKER_USER = "your-dockerhub-username"
    }
    
    stages {
        stage('Build & Push Frontend') {
            steps {
                dir('frontend') {
                    script {
                        sh 'docker build -t ${DOCKER_USER}/frontend-image .'
                        sh 'docker push ${DOCKER_USER}/frontend-image'
                    }
                }
            }
        }
        
        stage('Build & Push Backend') {
            steps {
                dir('backend') {
                    script {
                        sh 'docker build -t ${DOCKER_USER}/backend-image .'
                        sh 'docker push ${DOCKER_USER}/backend-image'
                    }
                }
            }
        }
    }
}

🌍 Deploying on AWS EC2

🔹 SSH into EC2

ssh -i your-key.pem ubuntu@your-ec2-instance-ip

🔹 Run Docker Containers

docker pull your-dockerhub-username/frontend-image
docker pull your-dockerhub-username/backend-image

docker run -d -p 3000:3000 your-dockerhub-username/frontend-image
docker run -d -p 5000:5000 your-dockerhub-username/backend-image

🎯 Conclusion

This project demonstrates a complete CI/CD workflow using Jenkins, Docker, and AWS EC2. The frontend and backend are containerized and deployed efficiently with an automated pipeline.

🌟 Happy Coding! 🚀
