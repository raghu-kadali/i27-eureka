pipeline {
    //where application run
    agent {
        label 'java-slave'
    }
    //tools
    tools {
        maven "maven-3.8.9"
    }
    //environment
    environment {
        APPLICATION_NAME ="eureka"
    }
    stages {
        stage('Build') {
            steps {
                echo "This is ${env.APPLICATION_NAME} Application"
                sh "mvn clean package -DskipTests=true" //if not test take moretime
            }
        }
        stage('sonar') {
            steps {
                echo "*********this is codequality stage**************"
                sh """
                mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=i27-eureka \
                    -Dsonar.host.url=http://34.60.63.221:9000 \
                    -Dsonar.login=sqp_add51cc7b42a4a1f48aa59a7c4f1b12099c8b77a

                """
            }
        }
    }
}