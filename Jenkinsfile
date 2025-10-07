pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('doc-hub-cred') // Jenkins credential ID
        IMAGE_NAME = "shiva8890/docker-01"
        IMAGE_TAG = "1"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "📦 Checking out source code..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🔧 Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "🔑 Logging in to Docker Hub..."
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "📤 Pushing Docker image to Docker Hub..."
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            echo "🚪 Cleaning up..."
            sh "docker logout || true"
            sh "docker image prune -af || true"
        }
        success {
            echo "✅ Docker image built and pushed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs for details."
        }
    }
}
