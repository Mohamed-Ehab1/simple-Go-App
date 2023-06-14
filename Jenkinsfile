pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockercredentials', variable: 'DOCKER_HUB_CREDENTIALS')]) {
                    script {
                        def imageName = "me2o01/goapp"
                        def imageTag = "v1"
                        def dockerFilePath = "/project"
                        def dockerRegistryUrl = "https://hub.docker.com/repository/docker/me2o01/goapp/"
                        
                        // Build the Docker image
                        sh "docker build -t ${imageName}:${imageTag} -f ${dockerFilePath} ."
                        
                        // Check if the Docker image build was successful
                        if (env.BUILD_STATUS == "SUCCESS") {
                            echo "Docker image build successful: ${imageName}:${imageTag}"
                            
                            // Log in to the Docker registry
                            sh "docker login -u me2o01 -p ${DOCKER_HUB_CREDENTIALS}"
                            
                            // Push the Docker image to the registry
                            sh "docker push ${imageName}:${imageTag}"
                            
                            echo "Docker image pushed to ${dockerRegistryUrl}/${imageName}:${imageTag}"
                        } else {
                            error "Docker image build failed"
                        }
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
