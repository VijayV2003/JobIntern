pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')  // Jenkins DockerHub credentials ID
        IMAGE_FRONTEND = "vijayv2003/sihfrontend-app"
        IMAGE_BACKEND = "vijayv2003/sihbackend-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out the source code..."
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "Building frontend and backend Docker images..."
                sh "docker build -t ${IMAGE_FRONTEND}:${IMAGE_TAG} ./frontend"
                sh "docker build -t ${IMAGE_BACKEND}:${IMAGE_TAG} ./backend"
            }
        }

        stage('Docker Hub Login') {
            steps {
                echo "Logging into Docker Hub..."
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
            }
        }

        stage('Push Docker Images to Hub') {
            steps {
                echo "Pushing images to Docker Hub..."
                sh "docker push ${IMAGE_FRONTEND}:${IMAGE_TAG}"
                sh "docker push ${IMAGE_BACKEND}:${IMAGE_TAG}"
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                echo "Deploying containers using docker-compose..."
                sh '''
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d --remove-orphans
                '''
            }
        }
    }

    post {
        always {
            echo "Cleanup..."
            sh '''
                docker logout
            '''
        }
    }
}
