# Build stage
FROM gradle:8.5-jdk17-alpine as builder
WORKDIR /app
COPY ./ ./
RUN gradle clean build --no-daemon

# Application stage
FROM openjdk:17.0-slim
WORKDIR /app
# Ensure the JAR file is correctly built and located at the specified path
COPY --from=builder /app/build/libs/test17-0.0.1-SNAPSHOT.jar /app/
EXPOSE 8080
USER nobody
ENTRYPOINT ["java", "-jar", "test17-0.0.1-SNAPSHOT.jar"]