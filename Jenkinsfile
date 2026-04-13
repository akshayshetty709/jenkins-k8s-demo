
pipeline {
    agent any

    environment {
        MINIKUBE_HOME = "/home/${env.USER}/.minikube"
        KUBECONFIG = "/home/${env.USER}/.kube/config"
        IMAGE_TAG = "demo-app:${BUILD_NUMBER}"
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
                sh 'docker build -t ${IMAGE_TAG} .'
            }
        }

        stage('Load Image to Minikube') {
            steps {
                echo '📦 Minikube- Image ...'
                sh 'minikube image load ${IMAGE_TAG} --profile minikube'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Kubernetes- Deploy ...'
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl set image deployment/demo-app demo-app=${IMAGE_TAG}'
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
