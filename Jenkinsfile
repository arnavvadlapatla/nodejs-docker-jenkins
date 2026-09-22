pipeline {
    agent any
    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nodejs-bluegreen:${BUILD_NUMBER} .'
            }
        }
        stage('Deploy Green') {
            steps {
                sh 'docker stop green || exit 0'
                sh 'docker rm green || exit 0'
                sh 'docker run -d -p 3002:3000 --name green nodejs-bluegreen:${BUILD_NUMBER}'
            }
        }
        stage('Test Green') {
            steps {
                sh 'curl -f http://localhost:3002/status'
            }
        }
        stage('Switch to Green') {
            steps {
                sh 'docker stop blue || exit 0'
                sh 'docker rm blue || exit 0'
            }
        }
    }
}