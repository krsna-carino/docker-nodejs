pipeline {
    agent {
        docker {
            image 'docker:24.0'   // Docker CLI image
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount host Docker daemon
        }
    }

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

        stage('Verify Docker Access') {
            steps {
                echo "🔍 Checking Docker version and permissions..."
                sh '''
                    docker version
                    docker info | grep -i "server"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "🔐 Logging in to DockerHub..."
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
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
