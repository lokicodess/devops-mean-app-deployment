pipeline {
    agent any

    environment {
        DOCKERHUB_FRONTEND = "lokesh3721/angular-app"
        DOCKERHUB_BACKEND  = "lokesh3721/backend"
        APP_SERVER = "ubuntu@13.233.108.247"
        APP_FOLDER = "devops-mean-app-deployment"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/lokicodess/devops-mean-app-deployment.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh "docker build -t $DOCKERHUB_FRONTEND ./frontend"
                sh "docker build -t $DOCKERHUB_BACKEND ./backend"
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKERHUB_FRONTEND
                        docker push $DOCKERHUB_BACKEND
                        docker logout
                    """
                }
            }
        }

        stage('Deploy to App EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $APP_SERVER '
                            cd $APP_FOLDER &&
                            docker compose pull &&
                            docker compose down &&
                            docker compose up -d
                        '
                    """
                }
            }
        }
    }
}
