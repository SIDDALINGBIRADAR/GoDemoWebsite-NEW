pipeline {
  agent {
    kubernetes {
      yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /busybox/cat
    args:
    - "0"
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: kaniko-secret
      items:
      - key: .dockerconfigjson
        path: config.json
'''
    }
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', 
             url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'
      }
    }
    stage('Build and Push') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --context `pwd` \
              --dockerfile=Dockerfile \
              --destination=docker.io/siddaling/go-web-site:latest \
              --verbosity=info \
              --insecure \
              --skip-tls-verify
          '''
        }
      }
    }
  }
}
