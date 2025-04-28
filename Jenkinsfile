pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // DockerHub credentials ID
        IMAGE_NAME = "vijayv2003/jobyintern"              // Replace with your actual DockerHub repo
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }
        stage('Docker Build') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }
        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub..."
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }
        stage('Docker Push') {
            steps {
                echo "Pushing Docker Image to DockerHub..."
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }
}
