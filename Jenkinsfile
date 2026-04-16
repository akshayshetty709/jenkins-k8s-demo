
pipeline {
    agent any

    environment {
        MINIKUBE_HOME = "/${env.USER}/.minikube"
        KUBECONFIG = "/${env.USER}/.kube/config"
        IMAGE_TAG = "demo-app:latest"
    }

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
                sh 'docker rmi demo-app:latest --force || true'
                sh 'docker build -t demo-app:latest .'
            }
        }

        stage('Load Image to Minikube') {
            steps {
                echo '📦 Minikube- Image ...'
                sh 'minikube image load demo-app:latest --profile minikube'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Kubernetes- Deploy ...'
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl rollout restart deployment demo-app'
                sh 'kubectl rollout status deployment demo-app'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Pods Running-...'
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
