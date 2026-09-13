pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        CI    = 'true'
        PATH  = "/usr/local/bin:${env.PATH}"
        IMAGE = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
        PORT  = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'npm install' }
        }
        stage('Test') {
            steps { sh 'npm test' }
        }
        stage('Docker build') {
            steps { sh "docker build -t ${IMAGE}:v1.0 ." }
        }
        stage('Deploy') {
            steps {
                sh "docker rm -f ${IMAGE} || true"
                sh "docker run -d --name ${IMAGE} -p ${PORT}:3000 ${IMAGE}:v1.0"
            }
        }
    }
}
