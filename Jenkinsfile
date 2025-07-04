pipeline {
    environment {
        dockerimagename = 'siddalingbiradar/go-website'
        dockerImage = ''
    }

    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Balaganesh15M/jenkins.git'
            }
        }

        stage('Go Build') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerhublogin'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploying App to Kubernetes') {
            steps {
                script {
                    sh '''
        echo 🚀 Deploying to Minikube...
        kubectl config use-context minikube
        kubectl apply -f deployment.yaml
        kubectl rollout status deployment/userapi
      '''
                }
            }
        }

    }
}
