pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('doc-hub-cred') // Jenkins Docker Hub credentials
    }
    stages {
        stage('SCM Checkout') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'master', url: 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }

        stage('Verify Docker Access') {
            steps {
                echo "🔧 Verifying Docker access..."
                sh 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🛠 Building Docker image..."
                sh 'docker build -t shivasrk/docker-pipeline:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "🔑 Logging in to Docker Hub..."
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "📤 Pushing Docker image to Docker Hub..."
                sh 'docker push shivasrk/docker-pipeline:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            echo "🚪 Logging out from Docker Hub..."
            sh 'docker logout || true'
        }
    }
}
