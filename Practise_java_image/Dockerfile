# Example #2 — Java App with Maven

# “In a Java microservices environment, we used a multi-stage build where Maven compiled and packaged the application in the first stage,
#  and a small JRE-based image was used in the second stage. This reduced the final image size significantly and speeded up CI/CD workflows.”


# Stage 1 - Maven build
FROM maven:3.9.4-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2 - JRE runtime
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]


