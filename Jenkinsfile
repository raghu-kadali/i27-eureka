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
                echo "*** Building Docker image and pushing to registry"
                sh "cp target/i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"
                sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT ./.cicd"
                echo "*** logging into docker registry***"
                sh "docker login -u ${DOCKER_CREDENTIALS_USR} -p ${DOCKER_CREDENTIALS_PSW}"
                echo "*** pushing docker image to registry***"
                sh "docker push ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT"
            }
        }

        stage('Deploy to dev env') {
            steps {
                echo "*** Deploying Docker image to development environment"
                // fisrt connect slave to dev vm because deployment done that place only docker pull and run command execute in dev vm
                // witcredentials actully pic from pipeline script to enter and add variable and pass to sh command
                withCredentials([usernamePassword(credentialsId: 'dev_madhu_creds', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                   // sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"whoami\""
                  // fisr time run: sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker run -d -p 8761:8761 --name ${env.APPLICATION_NAME} -d ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT\""
                   // above command one time run second time run it will give error because container name is same so in realtime multiple time somechnages ecerytime not change image right so we can add one parameter stop,remove,recerate container if exist and run new one no error in docker only
                   // in realtime k8s deployment used because new changes  depaloyment convert new vesrion rs con paa etc
                   // stop the container
                   sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker stop ${env.APPLICATION_NAME}-dev\""
                   // remove the container
                   sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker remove ${env.APPLICATION_NAME}-dev\""
                     // run the container
                     sh "sshpass -p $PASSWORD -v ssh -o StrictHostKeyChecking=no $USERNAME@$docker_vm_ip_address \"docker run -d -p 8761:8761 --name ${env.APPLICATION_NAME}-dev -d ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT\""


                }
            }
        }
    }
}

    //     stage('Deploy to dev env') {
    //         steps {
    //             echo "*** Deploying Docker image to development environment"
    //             // step1: connect js to dev env vm first
    //             sshpass -p -v ssh -o strictHostKeyChecking=no username@ipadress
    //             withCredentials([usernamePassword(credentialsId: 'dev_madhu_creds', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
    //                 sh "sshpass -p ${PASSWORD} ssh -o StrictHostKeyChecking=no ${USERNAME}@"
    //         }
    //     }
    // }
    
    

    





























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