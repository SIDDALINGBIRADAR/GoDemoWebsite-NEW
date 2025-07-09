[16:01] Bala Ganesh M
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

    - --dockerfile=Dockerfile

    - --context=.

    - --destination=docker.io/siddaling/go-web-site:latest

    - --verbosity=info

    - --insecure

    - --skip-tls-verify

    volumeMounts:

      - name: docker-config

        mountPath: /kaniko/.docker/

        readOnly: true

  restartPolicy: Never

  volumes:

    - name: docker-config

      secret:

        secretName: kaniko-secret

        items:

          - key: .dockerconfigjson

            path: config.json

"""

    }

  }
 
  stages {

    stage('Checkout') {

      steps {

        git branch: 'main',

            url: 'https://github.com/SIDDALINGBIRADAR/GoDemoWebsite-NEW.git'

      }

    }
 
    stage('Build and Push Docker Image') {

      steps {

        container('kaniko') {

          echo "Docker image is being built and pushed using Kaniko"

        }

      }

    }

  }

}

 
