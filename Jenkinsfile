pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("vijayv2003/sihfrontend-app:latest", "./frontend")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("vijayv2003/sihfrontend-app:latest").push()
                    }
                }
            }
        }
    }
}
