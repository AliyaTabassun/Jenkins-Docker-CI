
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "abh-app"
        DOCKER = "C:\\Users\\STUDENT\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe"
        PYTHON = "C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python311\\python.exe"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                bat '"%PYTHON%" app.py'
            }
        }

        stage('3. Build Docker Image') {
            steps {
                bat '"%DOCKER%" build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% .'

                bat '"%DOCKER%" tag %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }

        stage('4. Push Docker Image') {
            steps {
                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
            echo "Latest image: ${REGISTRY}/${IMAGE_NAME}:latest"
        }

        failure {
            echo "Pipeline failed. Check Console Output for the error."
        }
    }
}
