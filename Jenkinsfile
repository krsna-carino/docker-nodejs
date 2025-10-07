pipeline {
    agent any   // Runs on your Jenkins node/container with Docker access

    environment {
        DOCKERHUB_CREDENTIALS = credentials('doc-hub-cred') // Your Docker Hub credentials ID in Jenkins
        IMAGE_NAME = "shiva8890/docker-01"                   // Change to your Docker Hub repo
        IMAGE_TAG  = "1"                                     // Tag for the image
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'master', url: 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🔧 Building Docker image..."
                sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    docker images
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "🔐 Logging into Docker Hub..."
                sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "🚀 Pushing Docker image to Docker Hub..."
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            echo "🚪 Cleaning up..."
            sh """
                docker logout || true
                docker image prune -af || true
            """
        }
        success {
            echo "✅ Docker image built and pushed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs for details."
        }
    }
}
