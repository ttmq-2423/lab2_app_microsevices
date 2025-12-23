pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        DOCKER_USER = credentials('dockerhub-username')
        DOCKER_PASS = credentials('dockerhub-password')
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                '''
            }
        }

        stage('Build frontend') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/frontend:$IMAGE_TAG ./frontend
                docker push $DOCKER_USER/frontend:$IMAGE_TAG
                '''
            }
        }

        stage('Build auth-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/auth-api:$IMAGE_TAG ./auth-api
                docker push $DOCKER_USER/auth-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build todos-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/todos-api:$IMAGE_TAG ./todos-api
                docker push $DOCKER_USER/todos-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build users-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/users-api:$IMAGE_TAG ./users-api
                docker push $DOCKER_USER/users-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build log-message-processor') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/log-message-processor:$IMAGE_TAG ./log-message-processor
                docker push $DOCKER_USER/log-message-processor:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Build & push all services successfully!"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
