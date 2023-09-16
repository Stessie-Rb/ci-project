#FROM jenkins/jenkins:latest
FROM jenkins/jenkins:lts-jdk11
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
ENV CASC_JENKINS_CONFIG /var/jenkins_home/casc.yaml
COPY casc.yaml /var/jenkins_home/casc.yaml

COPY --chown=jenkins:jenkins plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

USER root

# Install Docker from official repo
RUN apt-get update && apt-get install curl -y
RUN curl -fsSl https://get.docker.com | sh
RUN curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose \
&& chmod +x /usr/local/bin/docker-compose
RUN groupmod -g 1001 docker
RUN usermod -aG docker jenkins
RUN newgrp docker 

RUN apt-get update && apt-get install nodejs -y

USER jenkins

EXPOSE 8080
EXPOSE 50000