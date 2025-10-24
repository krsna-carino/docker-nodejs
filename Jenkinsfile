pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker-credits')
        IMAGE_NAME = "krsna0707/docker-pipeline"
    }
    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/krsna-carino/docker-nodejs.git'
            }
        }
        stage('Verify Docker Access') {
            steps { sh 'docker info' }
        }
        stage('Build Docker Image') {
            steps { sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ." }
        }
        stage('Login to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                '''
            }
        }
        stage('Push Docker Image') {
            steps { sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}" }
        }
    }
    post {
        always { sh 'docker logout || true' }
    }
}
