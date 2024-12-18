pipeline {
    agent any
    environment {
        REGISTRY = 'your-docker-registry'
        IMAGE_NAME = 'your-app'
        KUBECONFIG_CREDENTIALS = credentials('kubeconfig')
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
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
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
