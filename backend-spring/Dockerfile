# Use the official Maven image
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
# TODO: environment variables for version and port (maybe binding to spring boot yml config possible?)
COPY --from=build /build/target/backend-spring-0.0.1-SNAPSHOT.jar /app/

# Copy the keystore file fore ssl cert
# TODO: vault (?) for cert password in application.yaml
# TODO: automate cert generation from certbot (cert expires 2024-09-28)
COPY /etc/letsencrypt/live/facemirrored.de/keystore.p12 /app/keystore.p12

EXPOSE 8088

CMD java -jar backend-spring-0.0.1-SNAPSHOT.jar