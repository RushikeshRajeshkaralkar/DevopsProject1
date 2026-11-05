pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'karalkarrushikesh21'
        IMAGE_NAME = "${DOCKERHUB_USER}/devopsproject1"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build React App') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_CREDENTIALS_USR',
                    passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW'
                )]) {
                    sh 'echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin'
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest'
                    sh 'docker push $IMAGE_NAME:latest'
                    sh 'docker logout'
                }
            }
        }
    }
}