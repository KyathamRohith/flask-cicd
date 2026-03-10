pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "rohith1305/flask-cicd"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/KyathamRohith/flask-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: '93c470a0-e8fe-425c-8f55-932aae8919d4', url: '']) {
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }

       stage('Deploy to Kubernetes') {
    steps {
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                sh 'kubectl set image deployment/flask-app flask-app=$DOCKER_IMAGE:$DOCKER_TAG'
        }
    }
}

    }
}
