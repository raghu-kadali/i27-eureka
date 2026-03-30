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
        DOCKER_HUB = "docker.io/dockerhubraghu"
        DOCKER_CREDENTIALS = credentials('raghu_dockerhub_creds')
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

        stage('formatBuild') {
            steps {
                echo "*** Formatting code using Spotless"
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    dockerBuildandPush().call()
                }
            }
        }

        stage('Deploy to dev env') {
            steps {
                script {
                    dockerDeploy('dev',5666).call()
                }
                
            }
        }

        stage('Deploy to test env') { // but its fail above dev on;ly exisisting cotainer remove,stop and create  incase first time container create  get error like these how 
              // so we solve by using some condition called try catch block in sh command to handle error and continue execution.
            steps {
               script {
                    dockerDeploy('test',5661).call()
                }
            }
        }

        stage('Deploy to stagee env') { // but its fail above dev on;ly exisisting cotainer remove,stop and create  incase first time container create  get error like these how 
              // so we solve by using some condition called try catch block in sh command to handle error and continue execution.
            steps {
                script {
                      dockerDeploy('stage',5662).call()
                 }
            }
        }

        stage('Deploy to prod') { // but its fail above dev on;ly exisisting cotainer remove,stop and create  incase first time container create  get error like these how 
              // so we solve by using some condition called try catch block in sh command to handle error and continue execution.
            steps {
                script {
                      dockerDeploy('prod',5663).call()
                 }
            }
        }
        
    }
}

// define method
//envDeploy,port is a variable
def dockerDeploy(envDeploy,port) {
    return {
        echo "*** Deploying Docker image to test environment"
            
                withCredentials([usernamePassword(credentialsId: 'dev_madhu_creds', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                  
                   script {
                    try {
                    // stop the container
                   sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker stop ${env.APPLICATION_NAME}-${envDeploy}\""
                   // remove the container
                   sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker rm ${env.APPLICATION_NAME}-${envDeploy}\""
                    } 
                    catch (error) {
                        echo "Container ${env.APPLICATION_NAME}-${envDeploy} does not exist, skipping stop and remove steps."
                    }
                    // craete and run the container
                   sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker run --name ${env.APPLICATION_NAME}-${envDeploy} -p $port:8761 -d ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT\""
                   }

                }
    }
}
    
// we can also simply write docker build and push just mention metond and call
def dockerBuildandPush() {
    return {
        echo "*** Building Docker image and pushing to registry"
        sh "cp target/i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"
        sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT ./.cicd"
        echo "*** logging into docker registry***"
        sh "docker login -u ${DOCKER_CREDENTIALS_USR} -p ${DOCKER_CREDENTIALS_PSW}"
        echo "*** pushing docker image to registry***"
        sh "docker push ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT"
    }
}
    





























// pipeline {
//     environment {
//         Application _Name = 'Eureka'
//        POM_VERSION = readMavenPom().getVersion() //read pom and fetch the version that stores in one vatrible
//        POM_PACKAGING =readMavenPom().getPackaging() //read pom and fetch the packaging that stores in one vatrible
//     }
//     stages {
//         stage('FormatBuild') {
//             // existsing i27-eureka-0.0.1-SNAPSHOT.jar
//             // Destination: i27-eureka-buildnumber-brachname.jar
//             steps {
//                 echo " testing existing jar is i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING}"
//                 echo "testing jar destination is i27-${env.APPLICATION_NAME}-${env.BUILD_NUMBER}-${env.BRANCH_NAME}.${env.POM_PACKAGING}"
              
//             }
//         }
//     }
// }