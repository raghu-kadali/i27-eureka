pipeline {
    agent 'jenkin-slave'
    tools {
        maven "maven-3.8.9"
        JDK "jdk-17"
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