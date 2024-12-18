# Node.js CI/CD Pipeline with Jenkins, Docker, and Kubernetes

## Overview

This repository demonstrates a CI/CD pipeline for a Node.js application using Jenkins, Docker, and Kubernetes. It automates:

1. **Running tests** automatically on pull requests.
2. **Building a Docker image** of the Node.js app.
3. **Deploying the app** to a Kubernetes cluster.
4. **Sending Slack notifications** about deployment status.

## Steps

1. **Jenkinsfile**: Defines the pipeline with stages for checking out code, running tests, building and pushing Docker images, and deploying to Kubernetes.
2. **Dockerfile**: Builds a Docker image for the Node.js app.
3. **Kubernetes Configs**: Configures Kubernetes deployment and service.
4. **Slack Notifications**: Sends notifications upon deployment success or failure.

To enable Slack notifications, you will need to add Jenkins credentials for Slack and Docker.

Slack: Use the Jenkins Slack plugin and create a Slack app for notifications. Add your Slack token to Jenkins credentials.
Docker: Add your Docker Hub credentials to Jenkins.

## Setup

1. Create a Jenkins job with the GitHub repository.
2. Configure Docker and Kubernetes credentials in Jenkins.
3. Configure the Slack plugin in Jenkins for notifications.

## Dependencies

- Jenkins
- Docker Hub
- Kubernetes (Minikube or Cloud)
- Slack (for notifications)
