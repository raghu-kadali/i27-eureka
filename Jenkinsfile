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
        SONAR_HOST_URL = "http://35.188.126.241:9000"
        SONAR_LOGIN_TOKEN = credentials('raghu_sonar_creds')
        // you craeet soem project in sonarqube server tehy craeet one toiken that tooken you can use and paste seceret text
    }

    stages {
        stage('Build') {
            steps {
                echo "***Building ${env.APPLICATION_NAME} application"
                sh "mvn clean package -DskipTests" // Run the Maven build command
            }
        }
        
        stage('sonar') {
            steps {
                echo "*** starting sonarqube analysis"
                // use wthsonqube method actually jenkins provide some method to integrate with sonarqube server so that we can run the sonar analysis in jenkins pipeline so that we can see the result in sonarqube server
                // withSonarQubeENV('http://35.188.126.241:9000') THIS IS NOT CORRECT WAY SO BELOW IS WE FOLLOW
                withSonarQubeEnv('SonarQubeServer') { // Use the SonarQube environment configured in Jenkins in system configuration in manage jenkins
                sh """ 
                    mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=i27-eureka \
                        -Dsonar.host.url=${env.SONAR_HOST_URL} \
                        -Dsonar.login=${env.SONAR_LOGIN_TOKEN}
                """ // Run the SonarQube analysis command
            }

            timeout(time: 1, unit: 'HOURS') { // Set a timeout for the SonarQube analysis stage
                waitForQualityGate abortPipeline: true // Wait for the SonarQube quality gate result and abort the pipeline if it fails
            }
        }

        stage('Docker Build and Push') {
            steps {
                echo "*** Building Docker image and pushing to registry"
            }

    }
}

}