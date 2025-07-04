pipeline {
    environment {
        dockerimagename = 'siddalingbiradar/go-website'
        dockerImage = ''
    }

    agent any

stage('Checkout Source') {
  steps {
    git branch: 'main', url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'
  }
}


        stage('Build image') {
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
