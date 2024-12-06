pipeline {
    agent any

    environment {
        // Define your Docker Hub credentials and image name here
        DOCKER_HUB_CREDS = credentials('docker-hub-credentials') // Docker Hub credentials
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

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub and push the image using docker.withRegistry
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDS) {
                        // Push the Docker image to Docker Hub
                        sh '''
                            docker push $DOCKER_IMAGE
                        '''
                    }
                }
            }
        }

        stage('Test Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        echo "DOCKER_USER: $DOCKER_USER"  // Debugging step
                        sh '''
                            docker login -u $DOCKER_USER -p $DOCKER_PASS
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
