pipeline { 
        agent any 
environment { 
    DOCKERHUB_USER = 'shivasrk' 
    IMAGE_NAME = 'shivasrk/test-nginx' 
    IMAGE_TAG = 'lts' 
} 
stages { 
    stage('Checkout') { 
        steps { 
            git branch: 'master', url: 
'https://github.com/kothapalli1094/docker-nodejs.git' 
        } 
    } 
    stage('Build JAR on Jenkins Host') { 
        steps { 
            echo "Building JAR on Jenkins host" 
            sh './mvnw clean package -DskipTests' 
        } 
    } 
    stage('Build Docker Image') { 
        steps { 
            echo "Building Docker image from runtime-only Dockerfile" 
            script { 
                
docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}") 
            } 
        } 
    } 
    stage('Push to DockerHub') { 
        steps { 
            echo "Pushing Docker image to Docker Hub" 
            script { 
                docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') { 
                    
docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}").p
ush() 
                } 
            } 
        } 
    } 
} 
} 
