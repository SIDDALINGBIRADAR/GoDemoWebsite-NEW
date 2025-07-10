pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: kaniko-job
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
    - name: workspace-volume
      mountPath: /workspace
  - name: jnlp
    image: jenkins/inbound-agent:3309.v27b_9314fd1a_4-1
    resources:
      requests:
        cpu: 100m
        memory: 256Mi
    volumeMounts:
    - name: workspace-volume
      mountPath: /home/jenkins/agent
  restartPolicy: Never
  volumes:
  - name: docker-config
    secret:
      secretName: dockerhub-secret
      items:
      - key: .dockerconfigjson
        path: config.json
  - name: workspace-volume
    emptyDir: {}
"""
        }
    }

    environment {
        IMAGE = "siddalingbiradar/userapi:latest"
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

         stage('Build with Kaniko') {
      steps {
        container('kaniko') {
          script {
            sh '''
              echo "📂 Listing current Kaniko working directory:"
              pwd
              echo "📂 Listing files in WORKSPACE:"
              ls -la ${WORKSPACE}
            '''
            sh '''
              /kaniko/executor \
                --dockerfile=Dockerfile \
                --context=${WORKSPACE} \
                --destination=docker.io/your-username/your-image:latest
            '''
          }
        }
      }
    }

    }
}
