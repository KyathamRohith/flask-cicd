pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/KyathamRohith/flask-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t rohith1305/flask-cicd:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: '93c470a0-e8fe-425c-8f55-932aae8919d4', url: '']) {
                    sh 'docker push rohith1305/flask-cicd:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'myid')]) {
                    withEnv(["KUBECONFIG=$HOME/.kube/config"]) {
                        sh 'kubectl apply -f k8s-deployment.yaml'
                    }
                }
            }
        }
    }
}
