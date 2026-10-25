
# syntax=docker/dockerfile:1

# Stage 1: Build
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /build

# Copy Maven configuration first for better layer caching
COPY app/springboot-app/pom.xml ./pom.xml

# Download dependencies
RUN --mount=type=cache,target=/root/.m2 \
    mvn -B -ntp dependency:go-offline

# Copy application source
COPY app/springboot-app/src/ ./src/

# Package the application and verify the JAR exists
RUN --mount=type=cache,target=/root/.m2 \
    mvn -B -ntp package -DskipTests \
    && test -n "$(find target -maxdepth 1 -type f -name '*.jar' ! -name '*.original' -print -quit)"

# Stage 2: Runtime
FROM eclipse-temurin:17-jre-alpine AS runtime

WORKDIR /app

# Create a non-root user and group
RUN addgroup -S -g 1001 app \
    && adduser -S -D -H -u 1001 -G app app

# Copy only the packaged JAR
COPY --from=builder --chown=1001:1001 \
    /build/target/*.jar /app/app.jar

# JVM container memory configuration
ENV JAVA_TOOL_OPTIONS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0"

EXPOSE 8082

# Run as non-root
USER 1001:1001

# Start the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
