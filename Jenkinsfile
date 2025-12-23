pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'your_dockerhub_username'
        DOCKERHUB_REPO = 'microservices'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-org/your-repo.git'
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

        stage('Build Images') {
            steps {
                sh '''
                docker-compose build
                '''
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag frontend        $DOCKER_USER/frontend:latest
                docker tag auth-api        $DOCKER_USER/auth-api:latest
                docker tag todos-api       $DOCKER_USER/todos-api:latest
                docker tag users-api       $DOCKER_USER/users-api:latest
                docker tag log-message-processor $DOCKER_USER/log-message-processor:latest
                '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker push $DOCKER_USER/frontend:latest
                docker push $DOCKER_USER/auth-api:latest
                docker push $DOCKER_USER/todos-api:latest
                docker push $DOCKER_USER/users-api:latest
                docker push $DOCKER_USER/log-message-processor:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI Pipeline finished successfully'
        }
        failure {
            echo '❌ CI Pipeline failed'
        }
    }
}
