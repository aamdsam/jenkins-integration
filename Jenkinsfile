pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                dockerContainer {
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
