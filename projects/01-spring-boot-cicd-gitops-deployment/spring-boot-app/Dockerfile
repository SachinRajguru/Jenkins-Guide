# Use lightweight and actively maintained Java 11 JRE image
FROM eclipse-temurin:11-jre

# Metadata
LABEL maintainer="sachin"
LABEL application="spring-boot-app"

# Application directory
WORKDIR /opt/app

# Build artifact
ARG artifact=target/spring-boot-web.jar

# Copy application JAR
COPY ${artifact} app.jar

# Start Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]