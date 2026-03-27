FROM eclipse-temurin:21-jdk
RUN mkdir -p /opt/i27
WORKDIR /opt/i27
COPY target/i27-eureka-0.0.1-SNAPSHOT.jar /opt/i27/i27-eureka.jar
EXPOSE 8761
ENTRYPOINT ["java", "-jar", "i27-eureka.jar"]