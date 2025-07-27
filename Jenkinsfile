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
        SONAR_URL ="http://34.60.63.221:9000"
        SONAR_TOKEN =credentials('sonar_creds')
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
            -Dsonar.host.url=${env.SONAR_URL}\
            -Dsonar.login=${env.SONAR_TOKEN}

        """
    }
}

    }
}





//
 /*steps {
                echo "*********this is codequality stage**************"
                sh """
                mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=i27-eureka \
                    -Dsonar.host.url=${env.SONAR_URL} \
                    -Dsonar.login=${env.SONAR_TOKEN}


                """
            }

//