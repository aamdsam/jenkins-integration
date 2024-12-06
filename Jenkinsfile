pipeline {
    agent any

    environment {
        // Define your Docker Hub credentials and image name here
        DOCKERHUB_CREDENTIALS = credentials('dockerhubcredentials') // Docker Hub credentials
        DOCKER_IMAGE = 'redheaven/hello-world:latest' // Image name
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

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using credentials securely
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        sh '''
                            echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        '''
                    }
                    echo 'Login Completed'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
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
                    sh '''
                        kubectl apply -f k8s/deployment.yaml -n $KUBERNETES_NAMESPACE
                    '''
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
