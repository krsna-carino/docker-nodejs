pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-jenkins')
        IMAGE_NAME = "kothapalli1094/nodeapp"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'master', url: 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }

        stage('Build JAR on Jenkins Host') {
            steps {
                echo "⚙️ Building JAR package using Maven..."
                // Make sure Maven is installed in Jenkins container
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh '''
                    docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                    docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest
                '''
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "🔑 Logging into Docker Hub..."
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                echo "⬆️ Pushing Docker image to Docker Hub..."
                sh '''
                    docker push $IMAGE_NAME:$BUILD_NUMBER
                    docker push $IMAGE_NAME:latest
                '''
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
