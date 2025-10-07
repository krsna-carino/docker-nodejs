pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('doc-hub-cred')
        
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'master', url: 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }


        stage('Build docker image') {
            steps {  
                sh 'docker build -t shiva8890/docker-01:$BUILD_NUMBER .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "🔑 Logging into Docker Hub..."
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Image to DockerHub') {
            steps{
                sh 'docker push shiva8890/docker-01:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            echo "🚪 Logging out and cleaning up..."
            sh 'docker logout'
        }
        success {
            echo "✅ Build and push successful! Image: $IMAGE_NAME:$BUILD_NUMBER"
        }
        failure {
            echo "❌ Build failed. Check Jenkins logs for details."
        }
    }
}
