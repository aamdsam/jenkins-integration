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
              - name: docker-k8s
                image: docker:20.10.24-dind
                args: ["--privileged"]
                command:
                - cat
                tty: true
                volumeMounts:
                - name: docker-sock
                  mountPath: /var/run/docker.sock
              - name: kubectl
                image: bitnami/kubectl:latest
                command:
                - cat
                tty: true
              volumes:
              - name: docker-sock
                hostPath:
                  path: /var/run/docker.sock
            """
        }
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }
        
        stage('Say Hello') {
            steps {
                echo 'Hello, World!'
            }
        }
        
        stage('Docker Info') {
            steps {
                echo 'Checking Docker info...'
                sh 'docker info'
            }
        }
        
        stage('Kubernetes Info') {
            steps {
                echo 'Checking Kubernetes info...'
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
