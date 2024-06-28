# Use the official Maven image with Eclipse Temurin JDK 22
FROM maven:3.8.7-openjdk-18 AS build

# Set the working directory inside the container
WORKDIR /build

# Copy the root pom.xml
COPY pom.xml .

# Run the mvn dependency:go-offline command to cache dependencies
RUN mvn dependency:go-offline

# Copy the backend-spring module's source code
COPY src ./src

# Run the mvn clean package command to build the application
RUN mvn clean package -DskipTests

# Runtime stage
FROM amazoncorretto:17

WORKDIR /app

# Copy build jar file to app dir
COPY --from=build /build/target/backend-spring-0.0.1-SNAPSHOT.jar /app/

EXPOSE 8088

CMD java -jar backend-spring-0.0.1-SNAPSHOT.jar