pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'siddalingbiradar/go-website'
        IMAGE_TAG = 'latest'
        REGISTRY_CREDENTIAL = 'dockerhub-creds'       // DockerHub credentials
        K8S_CREDENTIAL_ID = 'aks-kubeconfig'         // kubeconfig as secret file
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIAL) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy to Minikube') {
            steps {
                withCredentials([file(credentialsId: 'aks-kubeconfig', variable: 'KUBECONFIG')]) {
                    sh 'kubectl --kubeconfig=$KUBECONFIG get nodes'
                    sh 'kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml'
                }
            }
        }
    }
}
