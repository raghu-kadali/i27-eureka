FROM eclipse-temurin:21-jdk
RUN mkdir -p /opt/i27
WORKDIR /opt/i27
ARG JAR_SOURCE
COPY ${JAR_SOURCE} "/opt/i27/i27-eureka.jar
EXPOSE 8761
ENTRYPOINT ["java", "-jar", "i27-eureka.jar"]