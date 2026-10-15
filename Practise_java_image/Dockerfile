# Example #2 — Java App with Maven

# “In a Java microservices environment, we used a multi-stage build where Maven compiled and packaged the application in the first stage,
#  and a small JRE-based image was used in the second stage. This reduced the final image size significantly and speeded up CI/CD workflows.”

# I use Maven and JDK in the builder stage to compile the source code and package the application 
#into a JAR. The final stage uses only a Java runtime image, copies the JAR, and starts it with 
#java -jar. 
#This keeps build tools out of the production image and reduces its size."

# Stage 1: Build the Java application
FROM maven:3.9.4-eclipse-temurin-17 AS builder
# Uses Maven and Java 17 to compile and package the application

WORKDIR /app
# Sets /app as the working directory

COPY pom.xml .
# Copies Maven configuration and dependencies metadata first
# This helps Docker cache dependency-related layers

COPY src ./src
# Copies Java source code and resources into the builder stage

RUN mvn clean package -DskipTests
# Removes previous build artifacts, compiles the application,
# and packages it into a JAR file
# -DskipTests skips test execution during the build

# Stage 2: Run the packaged application
FROM eclipse-temurin:17-jre-alpine
# Uses a smaller Java Runtime Environment image without Maven

WORKDIR /app
# Sets /app as the runtime working directory

COPY --from=builder /app/target/*.jar app.jar
# Copies the generated JAR from the builder stage and renames it app.jar

EXPOSE 8080
# Documents the expected application port; adjust if needed

ENTRYPOINT ["java", "-jar", "app.jar"]
# Starts the Java application when the container launches
