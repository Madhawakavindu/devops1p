pipeline {
    agent any

    stages {

        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Madhawakavindu/devops1p'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t madhawa123/nodeapp_cuban:%BUILD_NUMBER% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pat', variable: 'DOCKER_PASS')]) {
                    bat "echo %DOCKER_PASS% | docker login -u madhawa123 --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push madhawa123/nodeapp_cuban:%BUILD_NUMBER%"
            }
        }
    }

    post {
        always {
            bat "docker logout"
        }
    }
}