# Use the official Jenkins image with JDK 21 as the base
FROM jenkins/jenkins:2.528.2-jdk21

# Switch to root user to install system packages
USER root

# Update package list and install lsb-release (needed for adding Docker repo)
RUN apt-get update && apt-get install -y lsb-release

# Download Docker's GPG key for package verification
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg

# Add Docker's official repository to apt sources
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list

# Update package list and install Docker CLI (not full daemon)
RUN apt-get update && apt-get install -y docker-ce-cli

# Switch back to Jenkins user for security
USER jenkins

# Install essential Jenkins plugins for Docker integration and Blue Ocean UI
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow json-path-api"
