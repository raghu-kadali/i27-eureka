pipeline {
    agent {
        label 'java-slave' // Agent label where pipeline will run
    }

    tools {
        maven 'maven-3.8.9' // Maven version
        jdk 'JDK-21'        // JDK version
    }

    environment {
        APPLICATION_NAME = 'Eureka'
        SONAR_HOST_URL = "http://35.188.126.241:9000"
        SONAR_LOGIN_TOKEN = credentials('raghu_sonar_creds')
        POM_VERSION = readMavenPom().getVersion() //read pom and fetch the version that stores in one vatrible
        POM_PACKAGING =readMavenPom().getPackaging() //read pom and fetch the packaging that stores in one vatrible
    }

    stages {
        stage('Build') {
            steps {
                echo "*** Building ${env.APPLICATION_NAME} application"
                sh "mvn clean package -DskipTests"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "*** Starting SonarQube analysis"
                withSonarQubeEnv('SonarQubeServer') { 
                    sh """
                        mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=i27-eureka \
                            -Dsonar.host.url=${env.SONAR_HOST_URL} \
                            -Dsonar.login=${env.SONAR_LOGIN_TOKEN}
                    """
                }
            }

            post {
                always {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                echo "*** Building Docker image and pushing to registry"
                sh "cp target/i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"
                sh "docker build --build-arg JAR_SOURCE=i27-eureka-0.0.1-SNAPSHOT.jar -t eureka:v4 ./.cicd"
                
            
            }
        }
    }
}