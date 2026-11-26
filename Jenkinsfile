
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "abh-app"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                bat '"C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python311\\python.exe" app.py'
            }
        }

        stage('3. Build Docker Image') {
            steps {
                bat 'docker build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% .'
                bat 'docker tag %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }

        stage('4. Push Docker Image') {
            steps {
                bat 'docker push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'
                bat 'docker push %REGISTRY%/%IMAGE_NAME%:latest'
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
