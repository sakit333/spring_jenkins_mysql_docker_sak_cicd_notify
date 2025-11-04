############################################################
# Spring Boot Dockerfile (Multi-Stage Build)
# Designed & Developed by @sak_shetty 🚀
############################################################

# === Stage 1: Build JAR with Maven ===
FROM maven:3.8.8-eclipse-temurin-17 AS builder
WORKDIR /app

# Copy Maven config & source
COPY pom.xml mvnw* ./
COPY .mvn .mvn
COPY src ./src

# Build application (skip tests for faster builds, enable tests in CI)
RUN mvn clean -DskipTests package

# === Stage 2: Runtime Image ===
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

# Copy built artifact from builder stage
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8088

# Default profile = prod
ENV SPRING_PROFILES_ACTIVE=prod

# Run application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
