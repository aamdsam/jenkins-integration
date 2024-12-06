pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: jenkins-agent
spec:
  containers:
  - name: docker
    image: docker:20.10.24-dind
    securityContext:
      privileged: true
    env:
    - name: DOCKER_TLS_CERTDIR
      value: ""
    args:
    - dockerd
    - --host=unix:///var/run/docker.sock
    - --host=tcp://0.0.0.0:2375
    ports:
    - containerPort: 2375
      name: docker
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock
    - name: docker-storage
      mountPath: /var/lib/docker

  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true

  volumes:
  - name: docker-sock
    emptyDir: {}
  - name: docker-storage
    emptyDir: {}
"""
        }
    }

    stages {
        stage('Docker Info') {
            steps {
                echo 'Checking Docker info...'
                sh 'docker -H unix:///var/run/docker.sock info'
            }
        }

        stage('Kubernetes Info') {
            steps {
                echo 'Checking Kubernetes version...'
                sh 'kubectl version --client'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
