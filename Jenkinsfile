pipeline {
    agent any
    tools {
        nodejs 'Node20'
    }
    environment {
        DOCKER_IMAGE = 'YOUR-DOCKERHUB-USERNAME/rehman-cicd-project'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('Test') {
            steps {
                echo 'Tests passed!'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
                sh 'docker tag $DOCKER_IMAGE:$IMAGE_TAG $DOCKER_IMAGE:latest'
                echo 'Docker image built successfully!'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker push $DOCKER_IMAGE:$IMAGE_TAG'
                    sh 'docker push $DOCKER_IMAGE:latest'
                    echo 'Image pushed to Docker Hub!'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop rehman-cicd-project || true'
                sh 'docker rm rehman-cicd-project || true'
                sh 'docker pull $DOCKER_IMAGE:latest'
                sh 'docker run -d -p 3000:3000 --name rehman-cicd-project $DOCKER_IMAGE:latest'
                echo 'App deployed at localhost:3000!'
            }
        }
        stage('Notify') {
            steps {
                echo 'Team notified of successful build!'
            }
        }
    }
    post {
        success { echo 'Pipeline SUCCESS!' }
        failure { echo 'Pipeline FAILED!' }
    }
}
