pipeline {
    agent none
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockercredentials')
        IMAGE_NAME = 'me2o01/goapp'
        IMAGE_TAG = 'v1'
        DOCKER_REGISTRY_URL = 'https://hub.docker.com/repository/docker/me2o01/goapp/'
        DOCKERFILE_PATH = 'project'
    }
    
    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push Docker Image') {
            agent any
            steps {
                script {
                    sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                    sh 'sh get-docker.sh'
                    sh 'usermod -aG docker jenkins'
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -f ${DOCKERFILE_PATH} ."
                    sh "docker login -u your-docker-username -p ${DOCKER_HUB_CREDENTIALS}"
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        
        failure {
            echo 'Pipeline failed'
        }
    }
}
 