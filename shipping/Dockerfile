FROM debian:10 AS build

# Install dependencies
RUN apt-get update && apt-get install -y curl tar

# Set Maven version
ENV MAVEN_VERSION=3.9.6

# Download and extract Maven manually
RUN curl -fsSL https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  | tar -xz -C /opt/ && \
  ln -s /opt/apache-maven-${MAVEN_VERSION} /opt/maven

# Add Maven to PATH
ENV PATH="/opt/maven/bin:$PATH"

WORKDIR /opt/shipping

# Rest of your commands below...
