pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shiva8890/docker-01:1"
        DOCKERHUB_CREDENTIALS = credentials('doc-hub-cred')
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'master', url: 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }

        stage('Ensure Docker Installed') {
            steps {
                echo "🔧 Checking and installing Docker if missing..."
                sh '''
                if ! command -v docker &> /dev/null; then
                    echo "Docker not found. Installing..."
                    apt-get update -y
                    apt-get install -y docker.io
                    echo "✅ Docker installed successfully."
                else
                    echo "✅ Docker is already installed."
                    docker --version
                fi
                '''
            }
        }

        stage('Build docker image') {
            steps {
                echo "🐳 Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "🔐 Logging in to DockerHub..."
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                echo "🚀 Pushing image to DockerHub..."
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }

    post {
        always {
            echo "🚪 Logging out and cleaning up..."
            sh '''
            docker logout || true
            docker image prune -af || true
            '''
        }
        success {
            echo "✅ Docker image built and pushed successfully!"
        }
        failure {
            echo "❌ Build failed. Check Jenkins logs for details."
        }
    }
}
