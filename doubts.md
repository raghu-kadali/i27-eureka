sh "cp target/i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"

it says everytime the jar version chnage when add new functionlity so that ime ho to pon pick teh latest version package 



#  sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:$GIT_COMMIT ./.cicd"
.cicd folder
i27-eureka-0.0.1.jar is here
        ↓
--build-arg JAR_SOURCE=i27-eureka-0.0.1.jar
        ↓
Dockerfile receives via ARG JAR_SOURCE
        ↓
COPY ${JAR_SOURCE} app.jar  // used to reuse the dockerfile 
        ↓
JAR is now app.jar INSIDE Docker image