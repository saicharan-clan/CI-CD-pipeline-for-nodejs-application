pipeline {
    agent any
    environment {
        REGISTRY = 'your-docker-registry'
        IMAGE_NAME = 'your-app'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE_NAME:${BUILD_NUMBER} .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $REGISTRY/$IMAGE_NAME:${BUILD_NUMBER}'
            }
        }
        stage('Set up Minikube') {
            steps {
                // Ensure Minikube is started
                sh 'minikube start'
                // Set environment variable for kubectl to use Minikube's context
                sh 'eval $(minikube -p minikube docker-env)'
            }
        }
        stage('Deploy to Minikube') {
            steps {
                // Apply deployment to Minikube using kubectl
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
    post {
        success {
            echo 'Deployment succeeded!'
            slackSend(channel: '#ci-cd', message: "Deployment succeeded: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
        failure {
            echo 'Deployment failed!'
            slackSend(channel: '#ci-cd', message: "Deployment failed: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
    }
}

