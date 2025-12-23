pipeline {
    agent any

    environment {
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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build & Push frontend') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/frontend:$IMAGE_TAG ./frontend
                docker push $DOCKER_USER/frontend:$IMAGE_TAG
                '''
            }
        }

        stage('Build & Push auth-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/auth-api:$IMAGE_TAG ./auth-api
                docker push $DOCKER_USER/auth-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build & Push todos-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/todos-api:$IMAGE_TAG ./todos-api
                docker push $DOCKER_USER/todos-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build & Push users-api') {
            steps {
                sh '''
                docker build -t $DOCKER_USER/users-api:$IMAGE_TAG ./users-api
                docker push $DOCKER_USER/users-api:$IMAGE_TAG
                '''
            }
        }

        stage('Build & Push log-message-processor') {
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
            echo "✅ All services built & pushed successfully!"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
