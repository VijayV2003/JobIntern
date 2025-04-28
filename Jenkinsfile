pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_FRONTEND = "01sudharsan/sihfrontend-app"
        IMAGE_BACKEND = "01sudharsan/sihbackend-app"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Create .env files') {
            steps {
                echo "Creating frontend and backend .env files..."

                dir('frontend') {
                    writeFile file: '.env', text: '''[your frontend env contents here]'''
                }

                dir('backend') {
                    writeFile file: '.env', text: '''[your backend env contents here]'''
                }
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                echo "Installing dependencies and building frontend/backend..."
                dir('frontend') {
                    sh 'npm config set registry https://registry.npmjs.org/'
                    sh 'npm install'
                }
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running tests on frontend and backend..."
                dir('frontend') {
                    sh 'npm test || true'
                }
                dir('backend') {
                    sh 'npm test || true'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "Building Docker images for frontend and backend..."

                dir('frontend') {
                    sh "docker build -t $IMAGE_FRONTEND:$IMAGE_TAG ."
                    sh "docker tag $IMAGE_FRONTEND:$IMAGE_TAG $IMAGE_FRONTEND:latest"
                }

                dir('backend') {
                    sh "docker build -t $IMAGE_BACKEND:$IMAGE_TAG ."
                    sh "docker tag $IMAGE_BACKEND:$IMAGE_TAG $IMAGE_BACKEND:latest"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..."
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing frontend and backend images to Docker Hub..."

                sh "docker push $IMAGE_FRONTEND:$IMAGE_TAG"
                sh "docker push $IMAGE_FRONTEND:latest"

                sh "docker push $IMAGE_BACKEND:$IMAGE_TAG"
                sh "docker push $IMAGE_BACKEND:latest"
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo "Deployment step placeholder - customize based on your target platform"
            }
        }
    }

    post {
        always {
            echo "Cleaning up Docker images locally..."
            sh 'docker logout || true'
            sh "docker rmi $IMAGE_FRONTEND:$IMAGE_TAG || true"
            sh "docker rmi $IMAGE_BACKEND:$IMAGE_TAG || true"
        }
    }
}
