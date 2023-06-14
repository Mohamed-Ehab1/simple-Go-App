pipeline {
    agent {
        docker {
            image 'docker:20.10.9'
            reuseNode true
        }
    }
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockercredentials')
        IMAGE_NAME = 'me2o01/goapp'
        IMAGE_TAG = 'v1'
        DOCKER_REGISTRY_URL = 'https://hub.docker.com/repository/docker/me2o01/goapp/'
        DOCKERFILE_PATH = 'project'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -f ${DOCKERFILE_PATH} ."
                    sh "docker login -u me2o01 -p ${DOCKER_HUB_CREDENTIALS}"
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
