#!/usr/bin/env groovy

pipeline {
    agent any
    
    stages {
        stage('Build App') {
            steps {
                echo "Building the application..."
            }
        }
        
        stage('Build Image') {
            steps {
                echo "Building the docker image..."
            }
        }

        stage('Deploy to AKS') {
            steps {
                // Use the Kubernetes CLI Plugin to handle the Kubeconfig securely
                // This replaces the need for AWS_ACCESS_KEY env vars
                withKubeConfig([credentialsId: 'my-aks-kubeconfig']) {
                    script {
                        echo 'Deploying to Azure Kubernetes Service... jenkins-jobs branch'

                        // Check if deployment exists or apply a file
                        // Using 'apply' is better practice than 'create' for CI/CD
                        // sh 'kubectl apply -f nginx.yaml'
                        // sh 'kubectl get nodes'
                        // sh 'kubectl get pods'

                        // Example of the command you used, updated for best practice:
                        sh 'kubectl create deployment nginx-deployment --image=nginx --dry-run=client -o yaml | kubectl apply -f - --validate=false'
                    }
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
