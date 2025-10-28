pipeline {
    agent any
    environment {
        IMAGE_NAME = "web"
        CONTAINER_NAME = "web1"
        HOST_PORT = "9090"
        CONTAINER_PORT = "80"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                  set -eux
                  docker build -t ${IMAGE_NAME} .
                '''
            }
        }
        stage('Remove Old Container') {
            steps {
                sh '''
                  set -eux
                  docker rm -f ${CONTAINER_NAME} || true
                '''
            }
        }
        stage('Run New Container') {
            steps {
                sh '''
                  set -eux
                  docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} --restart unless-stopped ${IMAGE_NAME}
                '''
            }
        }
    }
    post {
        success {
            echo "✅ Contenedor ${CONTAINER_NAME} corriendo en puerto ${HOST_PORT}"
        }
        failure {
            echo "❌ Error en el pipeline"
        }
    }
}
