pipeline {
    agent any
    stages {
        stage('Build Backend Image') {
            steps {
                // Removed CC_LAB-6/ prefix
                sh 'docker build -t backend-app backend'
            }
        }
        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker network create app-network || true
                docker rm -f backend1 backend2 || true
                docker run -d --name backend1 --network app-network backend-app
                docker run -d --name backend2 --network app-network backend-app
                '''
            }
        }
        stage('Deploy NGINX Load Balancer') {
            steps {
                // Removed CC_LAB-6/ prefix
                sh '''
                docker build -t nginx-lb nginx
                docker rm -f nginx-lb || true
                docker run -d --name nginx-lb -p 80:80 --network app-network nginx-lb
                '''
            }
        }
    }
}
