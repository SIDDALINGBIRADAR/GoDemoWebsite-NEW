pipeline {
    environment {
        dockerimagename = 'siddalingbiradar/go-website'
        dockerImage = ''
    }

    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'
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

        stage('Deploy to Minikube') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
                        sh 'kubectl --kubeconfig=$KUBECONFIG get nodes'
                        sh 'kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml'
                    }
                }
            }
        }
    }
}
