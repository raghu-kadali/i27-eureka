pipeline {
    agent {
        label 'java-slave' // Specify the agent label to run the pipelinn
    }

    tools {
        maven 'Maven-3.8.9' // Specify the Maven version to use
        jdk 'JDK 17' // Specify the JDK version to use
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh "mvn clean package" // Run the Maven build command
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // Add your test commands here
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy commands here
            }
        }
    }
}