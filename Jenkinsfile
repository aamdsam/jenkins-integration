pipeline {
    agent none // Tidak ada agen default; tiap stage akan menentukan agen
    stages {
        stage('Prepare Docker') {
            agent {
                docker {
                    image 'docker:24.0.2-dind' // Docker in Docker image
                    args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh 'docker --version' // Verifikasi Docker tersedia
            }
        }

        stage('Build Image') {
            agent {
                docker {
                    image 'docker:24.0.2-dind'
                    args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh 'docker build -t my-app:latest .' // Build image menggunakan Docker
            }
        }

        stage('Test Kubernetes CLI') {
            agent {
                docker {
                    image 'bitnami/kubectl:latest'
                    args '-v /root/.kube:/root/.kube'
                }
            }
            steps {
                sh 'kubectl version --client' // Verifikasi kubectl tersedia
            }
        }
    }
}
