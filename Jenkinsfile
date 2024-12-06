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
                hostPath:
                  path: /var/run/docker.sock
              - name: docker-storage
                emptyDir: {}
            """
        }
    }

    stages {
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
