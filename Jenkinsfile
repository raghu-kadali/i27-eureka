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
        SONAR_HOST_URL = 'http://35.188.126.241:9000'
        SONAR_LOGIN_TOKEN = credentials('raghu_sonar_creds')
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
                        -Dsonar.host.url=${env.SONAR_HOST_URL} \
                        -Dsonar.login=${env.SONAR_LOGIN_TOKEN}
                """ // Run the SonarQube analysis command
            }
        }

    }
}