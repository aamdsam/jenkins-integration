pipeline {
    agent { label 'testing' }  // Specify the built-in agent

    stages {
        stage('Check Docker and kubectl Installation') {
            steps {
                script {
                    // Check Docker version
                    try {
                        sh 'docker --version'
                        echo 'Docker is installed.'
                    } catch (Exception e) {
                        error 'Docker is not installed.'
                    }

                    // Check kubectl version
                    try {
                        sh 'kubectl version --client'
                        echo 'kubectl is installed.'
                    } catch (Exception e) {
                        error 'kubectl is not installed.'
                    }
                }
            }
        }
    }
}
