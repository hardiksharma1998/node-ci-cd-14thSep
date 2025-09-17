pipeline {
    agent any
    
    // Environment variables for Docker configuration.
    environment {
        DOCKER_IMAGE_NAME = 'hardikdockeraws/nodeproject'
    }
    
    stages {
        stage('Build Image') {
            steps {
                // Build the Docker image with the correct tag format.
                sh "docker build -t ${env.DOCKER_IMAGE_NAME}:latest ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                // Authenticate with Docker Hub using credentials stored in Jenkins.
                withCredentials([
                    usernamePassword(
                        credentialsId: 'd2545067-fea9-4300-b56c-f01049d15fe3',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )
                ]) {
                    sh "echo \$DOCKERHUB_PASSWORD | docker login -u \$DOCKERHUB_USERNAME --password-stdin"
                }
            }
        }
        
        stage('Push Image') {
            steps {
                // Push the image to Docker Hub.
                sh "docker push ${env.DOCKER_IMAGE_NAME}:latest"
            }
        }
        
       stage('Deploy Locally') {
            steps {
                // Stop any existing container to free up the port.
                echo 'Stopping existing container if it is running...'
                sh "docker stop nodeproject-app || true"
                
                // Remove the old container.
                echo 'Removing old container...'
                sh "docker rm nodeproject-app || true"
                
                // Run the new container from the pushed image.
                echo 'Deploying new container...'
                sh "docker run -d --name nodeproject-app -p 3000:8000 ${env.DOCKER_IMAGE_NAME}:latest"
            }
        }
    }
}
