pipeline {
    agent any
    
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
                withDockerRegistry([credentialsId: DOCKER_HUB_CREDENTIALS]) {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}", "-f ${DOCKERFILE_PATH} .")
                    docker.withRegistry("${DOCKER_REGISTRY_URL}") {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
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
