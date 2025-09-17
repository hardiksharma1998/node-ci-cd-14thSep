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
        
        stage('Deploy') {
            steps {
                // This stage is still in your pipeline, but we will update it
                // to deploy to AWS ECS Fargate once the push is successful.
                sh 'echo "Skipping deployment for now. We will deploy to AWS ECS Fargate next."'
            }
        }
    }
}
