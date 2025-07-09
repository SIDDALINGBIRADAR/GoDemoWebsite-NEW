pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    jenkins: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      command:
        - /busybox/cat
      tty: true
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
                git url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git', branch: 'main'
            }
        }
 
        stage('Build & Push Docker Image with Kaniko') {
            steps {
                container('kaniko') {
                    sh '''
                    /kaniko/executor \
                      --context=dir://workspace/ \
                      --dockerfile=Dockerfile \
                      --destination=docker.io/bala1115/go-kaniko-demo:latest
                    '''
                }
            }
        }
    }
}
