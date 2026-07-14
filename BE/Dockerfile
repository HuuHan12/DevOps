# Stage 1: Build the application using Gradle
FROM public.ecr.aws/docker/library/gradle:8.8-jdk17 AS build
WORKDIR /app

# Copy the build files first to leverage Docker cache
COPY build.gradle settings.gradle ./
COPY gradle ./gradle
COPY gradlew ./
COPY src ./src

# Build the application
RUN gradle build --no-daemon -x test

# Stage 2: Run the application
FROM public.ecr.aws/docker/library/openjdk:17-slim
WORKDIR /app
COPY --from=build /app/build/libs/*-SNAPSHOT.jar app.jar
EXPOSE 8085
ENTRYPOINT ["java", "-jar", "app.jar"]

