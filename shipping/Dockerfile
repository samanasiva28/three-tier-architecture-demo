# Stage 1: Build
FROM debian:10 AS build

# Install dependencies
RUN apt-get update && apt-get install -y curl tar openjdk-11-jdk

# Set Maven version
ENV MAVEN_VERSION=3.9.6

# Download and extract Maven manually
RUN curl -fsSL https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
    | tar -xz -C /opt/ && \
    ln -s /opt/apache-maven-${MAVEN_VERSION} /opt/maven

# Add Maven to PATH
ENV PATH="/opt/maven/bin:$PATH"

# Set working directory
WORKDIR /opt/shipping

# Copy source code
COPY . .

# Build the app
RUN mvn clean package -DskipTests

# Stage 2: Runtime
FROM openjdk:11-jre-slim

WORKDIR /app

# Copy built jar from previous stage
COPY --from=build /opt/shipping/target/*.jar app.jar

# Expose port (change if different)
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
                                            
