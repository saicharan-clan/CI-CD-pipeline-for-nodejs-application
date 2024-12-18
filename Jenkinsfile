pipeline {
    agent any

    environment {
        REGISTRY = 'docker.io'
        IMAGE_NAME = 'my-node-app'
        K8S_NAMESPACE = 'default'
        SLACK_CHANNEL = '#ci-cd'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Install dependencies and run tests
                    sh 'npm install'
                    sh 'npx jest --maxWorkers=4'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh """
                        docker build -t ${REGISTRY}/${IMAGE_NAME}:${GIT_COMMIT} .
                        docker tag ${REGISTRY}/${IMAGE_NAME}:${GIT_COMMIT} ${REGISTRY}/${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Login to Docker and push image
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
                            docker push ${REGISTRY}/${IMAGE_NAME}:${GIT_COMMIT}
                            docker push ${REGISTRY}/${IMAGE_NAME}:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use kubectl to deploy to Kubernetes
                    sh """
                        kubectl set image deployment/my-node-app my-node-app=${REGISTRY}/${IMAGE_NAME}:${GIT_COMMIT} -n ${K8S_NAMESPACE}
                    """
                }
            }
        }
    }

    post {
        success {
            slackSend(channel: SLACK_CHANNEL, message: "Deployment successful!")
        }

        failure {
            slackSend(channel: SLACK_CHANNEL, message: "Deployment failed!")
        }
    }
}
