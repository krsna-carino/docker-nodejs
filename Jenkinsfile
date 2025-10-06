pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('docker-jenkins')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git 'https://github.com/kothapalli1094/docker-nodejs.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t kothapalli1094/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push kothapalli1094/nodeapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}

