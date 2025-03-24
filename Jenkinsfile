pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('myid') // Use Jenkins stored kubeconfig
    }

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
                withCredentials([usernamePassword(credentialsId: 'myid', usernameVariable: 'KUBE_USER', passwordVariable: 'KUBE_PASS')]) {
                    withEnv(["KUBECONFIG=$HOME/.kube/config"]) {
                        // Authenticate with Kubernetes
                        sh 'kubectl config set-credentials admin --username=$KUBE_USER --password=$KUBE_PASS'
                        sh 'kubectl config set-context minikube --user=admin'
                        sh 'kubectl config use-context minikube'

                        // Apply Kubernetes deployment
                        sh 'sudo kubectl apply -f k8s-deployment.yaml'
                    }
                }
            }
        }
    }
}
