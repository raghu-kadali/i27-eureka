pipeline {
    agent 'java-slave'
    tools {
        maven "maven-3.8.9"
    }
    stages {
        stage('Build') {
            steps {
                echo "this is build stage"
                sh "mvn package"
            }
        }
    }
}