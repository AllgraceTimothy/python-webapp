pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-demo"
        CONTAINER_NAME = "flask-demoapp"
        APP_PORT = "5001"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build \
                -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p ${APP_PORT}:5000 \
                ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                docker ps
                '''
            }
        }
    }
}