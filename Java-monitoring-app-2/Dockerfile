# ===== Stage 1: Build the JAR =====
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set working dir
WORKDIR /app

# Copy pom.xml and dependencies first (for build cache)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the rest of the code
COPY src ./src

# Build the app
RUN mvn clean package -DskipTests

# ===== Stage 2: Run the app =====
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copy only the built JAR from the builder stage
COPY --from=builder /app/target/java-monitoring-app-0.0.1-SNAPSHOT.jar app.jar

# Expose Spring Boot default port
EXPOSE 9090

ENTRYPOINT ["java", "-jar", "app.jar"]
