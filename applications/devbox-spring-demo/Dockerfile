# Stage 1: Build the application
FROM maven:3.9.6-eclipse-temurin-22 AS build

WORKDIR /app

# Install curl to check internet connectivity
RUN apt-get update && apt-get install -y curl

# Test internet connectivity
RUN curl -I https://repo.maven.apache.org/maven2/ || exit 1

# Copy Maven dependencies for caching
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the source code
COPY src src

# Build the application and make sure the JAR is created
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:22
WORKDIR /app

# Copy the JAR from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose port 8080
EXPOSE 8080

# Run the JAR file
CMD ["java", "-jar", "app.jar"]
