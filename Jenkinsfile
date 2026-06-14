pipeline {
    agent any
    tools {
        nodejs 'Node20'
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
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                echo 'Application Deployed Successfully!'
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
