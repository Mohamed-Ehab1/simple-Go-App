pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "me2o01/goapp"
                    def imageTag = "v1"
                    def dockerFilePath = "./project"
                    def dockerRegistryUrl = "https://hub.docker.com/repository/docker/me2o01/goapp/"
                    def dockerCredentialsId = "dokercredentials"
                    // Build the Docker image
                    def dockerBuild = docker.build("${dockerRegistryUrl}/${imageName}:${imageTag}", "-f ${dockerFilePath} .")
                    
                    // Check if the Docker image build was successful
                    if (dockerBuild) {
                        echo "Docker image build successful: ${imageName}:${imageTag}"
                        docker.withRegistry(dockerRegistryUrl, dockerCredentialsId) {
                            dockerBuild.push()
                            echo "Docker image pushed to ${dockerRegistryUrl}/${imageName}:${imageTag}"
                        }
                    } else {
                        error "Docker image build failed"
                    }
                }
            }
        }
    }

}
