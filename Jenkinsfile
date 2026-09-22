pipeline {
    agent any
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
                sh 'docker stop green || true'
                sh 'docker rm green || true'
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
                sh 'docker stop blue || true'
                sh 'docker rm blue || true'
            }
        }
    }
}