pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // your Jenkins DockerHub credentials
        IMAGE_NAME = "vijayv2003/jobintern"              // YOUR dockerhub image (choose accordingly)
        IMAGE_TAG = "latest"                              // or use BUILD_NUMBER
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the code..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "Logging in to DockerHub..."
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing Docker image to DockerHub..."
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Deploy (Run Container)') {
            steps {
                echo "Deploying Docker container..."
                sh '''
                    docker stop jobintern-container || true
                    docker rm jobintern-container || true
                    docker run -d -p 5000:5000 --name jobintern-container $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace if needed."
        }
    }
}
