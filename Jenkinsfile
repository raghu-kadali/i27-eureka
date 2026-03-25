pipeline {
    agent {
        label 'java-slave' // Specify the agent label to run the pipelinn
    }

    tools {
        maven 'maven-3.8.9' // Specify the Maven version to use
        jdk 'JDK-21' // Specify the JDK version to use
    }

    environment {
        APPLICATION_NAME = 'Eureka' // Define an environment variable for the application name
    }

    stages {
        stage('Build') {
            steps {
                echo "***Building the ${env.APPLICATION_NAME} application"
                sh "mvn clean package" // Run the Maven build command
            }
        }
        
        stage('sonar') {
            steps {
                echo "*** starting sonarqube analysis"
                sh """ 
                mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=i27-eureka \
                        -Dsonar.host.url=http://35.188.126.241:9000 \
                        -Dsonar.login=sqp_77ecc4726276900444fbab2f6bf125f8c9724be1
                """ // Run the SonarQube analysis command
            }
        }

    }
}