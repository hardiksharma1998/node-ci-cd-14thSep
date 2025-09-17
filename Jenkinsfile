pipeline {
    agent any
    
    stages {
        stage('Build Image') {
            steps {
                sh 'docker build -t hardikdockeraws/nodeproject:latest .'
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                // Ensure your Docker Hub username and password are stored as
                // Secret Text credentials in Jenkins.
                // The Credentials ID should be 'dockerhub-credentials'.
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
                sh 'docker push hardikdockeraws/nodeproject:latest'
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
                sh "docker run -d --name nodeproject-app -p 3000:3000 ${env.DOCKER_IMAGE_NAME}:latest"
        }
    }
}
