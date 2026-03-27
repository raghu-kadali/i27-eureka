pipeline {
    agent {
        label 'java-slave'
    }

    tools {
        maven 'maven-3.8.9'
        jdk 'JDK-21'
    }

    environment {
        APPLICATION_NAME = 'eureka'
        SONAR_HOST_URL = "http://35.188.126.241:9000"
        SONAR_LOGIN_TOKEN = credentials('raghu_sonar_creds')
        POM_VERSION = readMavenPom().getVersion()
        POM_PACKAGING = readMavenPom().getPackaging()
        //
        DOCKERHUB = "docker.io/dockerhubraghu"
        DOCKERHUB_CREDENTILAS = credentials('raghu_dockerhub_creds')
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
                            -Dsonar.token=${env.SONAR_LOGIN_TOKEN}
                    """
                }
            }
            post {                              // ✅ Fixed — post at stage level
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
               // sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t eureka:v4 ./.cicd"
                 sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKERHUB}/${env.APPLICATION_NAME}:$GIT_COMMIT ./.cicd"
                 // git commit say pick the dynamic tak from github and use it as tag for docker image
                 echo ***************docker login************************************
                 sh "docker login -u ${env.DOCKERHUB_CREDENTILAS_USR} -p ${env.DOCKERHUB_CREDENTILAS_PSW}" //usr and psw fetch etra we write
                 echo ***************docker push************************************
                 sh "docker push ${env.DOCKERHUB}/${env.APPLICATION_NAME}:$GIT_COMMIT"
            }
        }
    }
}