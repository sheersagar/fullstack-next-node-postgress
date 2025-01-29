# 🚀 Node.js Full-Stack Application

This repository contains a full-stack Node.js application with separate frontend and backend components. The application is deployed on an AWS EC2 instance using Jenkins for CI/CD, and Docker images are pushed to Docker Hub.

## 🏗 Project Structure

```
📦 project-root
├── 📁 frontend    # React or any frontend framework
├── 📁 backend     # Express.js backend with Prisma and PostgreSQL
├── 📝 Jenkinsfile # CI/CD pipeline for automated deployment
├── 📜 README.md   # Project documentation
└── 📜 terraform/  # Terraform modules for EC2 provisioning
```

## 🛠️ Tech Stack
- **Frontend:** React.js (or any preferred frontend framework)
- **Backend:** Node.js with Express.js and Prisma ORM
- **Database:** PostgreSQL
- **Containerization:** Docker
- **CI/CD:** Jenkins
- **Infrastructure as Code:** Terraform
- **Cloud Provider:** AWS (EC2, EBS, IAM, Security Groups)

---

## 🌍 Getting Started
### Prerequisites
Ensure you have the following installed:
- [Node.js](https://nodejs.org/)
- [Docker](https://www.docker.com/)
- [Jenkins](https://www.jenkins.io/)
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- AWS CLI (configured with necessary permissions)

### Clone the Repository
```sh
git clone https://github.com/your-repo/nodejs-fullstack-app.git
cd nodejs-fullstack-app
```

### Environment Variables
Create a `.env` file in the backend folder with:
```
DATABASE_URL=postgresql://user:password@db_host:5432/db_name
PORT=5000
```

---

## 🏗️ Setting Up Jenkins Pipeline
Jenkins automates the build and deployment process.

### Add the Jenkinsfile
```groovy
pipeline {
    agent any
    tools {
        nodejs 'node23'
    }
    
    environment {
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
                steps {
                git branch: 'main', changelog: false, credentialsId: 'b45478de-ae02-424a-aa74-91dc28bc8902', poll: false, url: 'https://github.com/sheersagar/fullstack-next-node-postgress.git'
            }
        }
        
        
        stage('Installing dependencies for frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Building fornt end application') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }
        
        stage('Installing dependencies for backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Dockerize front end') {
            environment {
                DOCKER_USER = "vishv3432"
                APP_NAME = "front-end-nodejs"
                IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            }
            steps {
                dir('frontend') {
                    script {
                        sh 'docker build -t ${IMAGE_NAME} .'
                        def dockerImage = docker.image("${IMAGE_NAME}")
                        docker.withRegistry('https://index.docker.io/v1/', "docker-red") {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
        
        
        stage('Dockerize back end') {
            environment {
                DOCKER_USER = "vishv3432"
                APP_NAME = "back-end-nodejs"
                IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            }
            steps {
                dir('backend') {
                    script {
                        sh 'docker build -t ${IMAGE_NAME} .'
                        def dockerImage = docker.image("${IMAGE_NAME}")
                        docker.withRegistry('https://index.docker.io/v1/', "docker-red") {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
```

---

## 🚀 Deployment on AWS EC2
We use Terraform to provision the EC2 instance with Session Manager enabled and a 50GB EBS volume.

### Terraform Configuration (`terraform/main.tf`)
```hcl
module "ec2" {
  source  = "terraform-aws-modules/ec2-instance/aws"
  name    = "jenkins-server"
  ami     = "ami-12345678" # Use an Amazon Linux AMI
  instance_type = "t3.medium"
  key_name      = "your-keypair"
  root_block_device {
    volume_size = 50
  }
  iam_instance_profile = "EC2SessionManagerRole"
}
```
### Apply Terraform Configuration
```sh
cd terraform
terraform init
terraform apply -auto-approve
```

---

## 🎯 Running the Application
Once the pipeline runs successfully, the application is available on EC2.
- Frontend: `http://<EC2_PUBLIC_IP>:3000`
- Backend: `http://<EC2_PUBLIC_IP>:5000`

## 📜 License
This project is licensed under the MIT License.

## 🙌 Contributions
Feel free to fork the repository and submit a pull request!

## 📧 Contact
For any queries, reach out at [vishavdeshwal@gmail.com](mailto:vishavdeshwal@gmail.com).
