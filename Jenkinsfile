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
                bat 'docker build -t madhawa123/nodeapp_test:%BUILD_NUMBER% .'
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pat', variable: 'DOCKER_PASS')]) {
                    bat '''
                    docker login -u madhawa123 -p %DOCKER_PASS%
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push madhawa123/nodeapp_test:%BUILD_NUMBER%'
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}