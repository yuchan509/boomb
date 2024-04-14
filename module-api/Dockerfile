# Build stage
FROM gradle:8.5-jdk17-alpine as builder
WORKDIR /app
COPY ./ ./
RUN gradle clean build --no-daemon

# Application stage
FROM openjdk:17.0-slim
WORKDIR /app
COPY --from=builder /app/build/libs/test17-0.0.1-SNAPSHOT.jar .
EXPOSE 8080
USER nobody
ENTRYPOINT ["java", "-jar", "test17-0.0.1-SNAPSHOT.jar"]
