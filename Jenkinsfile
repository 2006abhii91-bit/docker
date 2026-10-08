pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "2006abhii91/docker-image"
    }
    stages {
        stage('Clone Reposiotory') {
            steps {
                git 'https://github.com/2006abhii91-bit/docker.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:v1")
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    bat "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:v1").push()
                    }
                }
            }
        }
    }
    post{
        success{
            echo "Docker image pushed successfully."
        }
        failure{
            echo "Docker image push failed."
        }
    }
}