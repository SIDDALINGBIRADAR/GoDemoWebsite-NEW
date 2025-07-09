pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /kaniko/executor
    args:
    - --context=git://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git
    - --dockerfile=Dockerfile
    - --destination=docker.io/bala1115/go-kaniko-demo:latest
    - --verbosity=debug
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker/
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
    stage('Build with Kaniko') {
      steps {
        echo 'Build started...'
        // No `sh` block needed. Kaniko runs from container's command.
      }
    }
  }
}
