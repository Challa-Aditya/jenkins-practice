pipeline {
    agent any

    environment{
        KUBECONFIG =  credentials('kubeconfig')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Challa-Aditya/jenkins-practice.git'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
        stage('Test Deployment') {
            steps {
                script {
                    def appStatus = sh(script: "kubectl get pods -l app=hello-app -o jsonpath='{.items[0].status.phase}'", returnStdout: true).trim()
                    if (appStatus != 'Running') {
                        error "Deployment failed"
                    }
                }
            }
        }
    }
}
