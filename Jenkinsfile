pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      args:
        - "--dockerfile=/workspace/Dockerfile"
        - "--context=dir://workspace/"
        - "--destination=docker.io/siddalingbiradar/go-kaniko-demo:latest"
      volumeMounts:
        - name: docker-config
          mountPath: /kaniko/.docker/
  restartPolicy: Never
  volumes:
    - name: docker-config
      secret:
        secretName: dockerhub-secret
        items:
          - key: .dockerconfigjson
            path: config.json
"""
        }
    }
 
    stages {
        stage('Clone Repo') {
            steps {
                git url: 'pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      args:
        - "--dockerfile=/workspace/Dockerfile"
        - "--context=dir://workspace/"
        - "--destination=docker.io/bala1115/go-kaniko-demo:latest"
      volumeMounts:
        - name: docker-config
          mountPath: /kaniko/.docker/
  restartPolicy: Never
  volumes:
    - name: docker-config
      secret:
        secretName: dockerhub-secret
        items:
          - key: .dockerconfigjson
            path: config.json
"""
        }
    }
 
    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/siddalingbiradar/go-kaniko-demo.git', branch: 'main'
            }
        }
 
        stage('Build & Push with Kaniko') {
            steps {
                container('kaniko') {
                    echo 'Building Docker image with Kaniko...'
                }
            }
        }
    }
}', branch: 'main'
            }
        }
 
        stage('Build & Push with Kaniko') {
            steps {
                container('kaniko') {
                    echo 'Building Docker image with Kaniko...'
                }
            }
        }
    }
}
