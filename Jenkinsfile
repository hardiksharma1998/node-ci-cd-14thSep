pipeline {
    agent any
    
    stages {
        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t my-node-app:latest .'
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    sh 'docker tag my-node-app:latest hardikdockeraws/my-node-app:latest'
                    sh 'docker push hardikdockeraws/my-node-app:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                sshagent(credentials: ['your-ssh-key-id']) {
                    sh 'ssh your-user@your-server-ip "docker stop my-node-app || true"'
                    sh 'ssh your-user@your-server-ip "docker rm my-node-app || true"'
                    sh 'ssh your-user@your-server-ip "docker run -d --name my-node-app -p 3000:3000 hardikdockeraws/my-node-app:latest"'
                }
            }
        }
    }
}