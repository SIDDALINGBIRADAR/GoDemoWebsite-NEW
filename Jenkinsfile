pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siddalingbiradar/go-website'
        TAG = 'latest'
        REGISTRY_CREDENTIAL = 'dockerhub-creds'
        KUBECONFIG_CRED = 'aks-kubeconfig'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: REGISTRY_CREDENTIAL, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push $IMAGE_NAME:$TAG
                    '''
                }
            }
        }

        stage('Deploy to AKS') {
            steps {
                withCredentials([file(credentialsId: KUBECONFIG_CRED, variable: 'KUBECONFIG')]) {
                    sh 'kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml'
                }
            }
        }
    }
}
