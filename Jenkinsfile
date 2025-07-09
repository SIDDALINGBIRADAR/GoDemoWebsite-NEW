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
    - /bin/sh
    args:
    - -c
    - "sleep infinity"
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: dockerhub-creds
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
            --destination=siddaling/go-web-site:latest \
            --verbosity=info
          '''
        }
      }
    }
  }
}
