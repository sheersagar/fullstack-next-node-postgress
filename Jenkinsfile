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
