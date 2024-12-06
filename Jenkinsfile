pipeline {
    agent any

    environment {
        // Define your Docker Hub credentials and image name here
        DOCKER_HUB_CREDS = credentials('docker-hub-credentials')
        DOCKER_IMAGE = 'redheaven/hello-world:latest'
        KUBE_CONTEXT = 'your-kube-context'  // Kube context if you have multiple clusters
        KUBERNETES_NAMESPACE = 'default'  // Replace with your namespace
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your repository
                git 'https://github.com/irfanrp/jenkins-integration.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the application using Maven
                    sh 'docker info'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh '''
                        docker build -t $DOCKER_IMAGE .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub and push the image
                    withDockerRegistry([credentialsId: DOCKER_HUB_CREDS]) {
                        sh '''
                            docker push $DOCKER_IMAGE
                        '''
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy to Kubernetes using kubectl
                    withKubeConfig([contextName: KUBE_CONTEXT]) {
                        sh '''
                            kubectl apply -f k8s/deployment.yaml -n $KUBERNETES_NAMESPACE
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up if necessary, for example, remove the Docker image locally
            sh 'docker rmi $DOCKER_IMAGE'
        }
    }
}
