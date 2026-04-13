pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo '📥 Code Pulling...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Docker Image Building...'
                sh 'docker build -t demo-app:latest .'
            }
        }

        stage('Load Image to Minikube') {
            steps {
                echo '📦 Minikube- Image Load ..'
                sh 'minikube image load demo-app:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Kubernetes- Deploy ..'
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Pods Running-ஆ Check பண்றோம்...'
                sh 'kubectl get pods'
                sh 'kubectl get service'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline Success! App is Live!'
        }
        failure {
            echo '❌ Pipeline Failed! Check logs.'
        }
    }
}
