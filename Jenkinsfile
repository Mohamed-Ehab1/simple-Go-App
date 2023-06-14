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
                        docker.image("docker").inside("-u root") {
                            docker.build("-t ${imageName}:${imageTag} -f ${dockerFilePath} .")
                        }
                        
                        // Check if the Docker image build was successful
                        if (env.BUILD_STATUS == "SUCCESS") {
                            echo "Docker image build successful: ${imageName}:${imageTag}"
                            
                            // Log in to the Docker registry
                            docker.withRegistry('https://registry.hub.docker.com', 'dockercredentials') {
                                // Push the Docker image to the registry
                                docker.image("${imageName}:${imageTag}").push()
                            }
                            
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
