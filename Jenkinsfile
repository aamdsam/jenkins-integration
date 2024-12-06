pipeline {
    agent none // Specify none because we'll use the docker block explicitly
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.9.3-eclipse-temurin-17'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn -B clean install'
            }
        }
    }
}
