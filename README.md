# Continuous integration workflow
The goal of this project is to develop a continuous integration workflow.
This workflow is based on a micro-service architecture, it is fully containerized using Docker. It contains a continuous integration server (Jenkins), a source code management solution (GitLab), a Docker private registry and differents monitoring tools. Real-time metrics solutions are used before and after deployment.
The workflow can be used to manage different types of applications. The Jenkins server is responsible for deploying the different environments (dev, prod, etc.), it launches the unit and functional tests, then sends and stores the results of these tests.
The configuration is fully editable through the Jenkinsfiles and Dockerfiles. To add a tool, you just have to modify the dockerfile, to modify the continuous integration pipeline of a project, you just have to modify the Jenkinsfile it contains.
A reverse proxy (Traefik) is used to route requests to the different services.

### Metrics collected before application deployment 
- Sonarqube:
The SonarQube tool is used to collect information on the code quality of applications before deployment.

### Metrics collected after application deployment 
- cAdvisor:
Exposes the metrics of Docker containers running on the server.
- node exporter:
Exposes server metrics.
- Prometheus:
Scans the different targets at a set interval to collect the metrcis.
- Prometheus metrics plugin installed in Jenkins:
Exposes a specific access point to allow Prometheus to collect metrics about the Jenkins CI solution.
- Grafana:
Displays as dashboards the metrcis stored by the defined data source (here Prometheus)

## Configure reverse proxy
Traefik provides access to a monitoring dashboard. Access to this dashboard is secured by an encrypted password. It is possible to use a utility like htpasswd contained in the apache2-utils package
```
    sudo apt-get install apache2-utils
```
Then generate the password with htpasswd. Replace your_password with the password you’d like to use:
```
    htpasswd -nb admin your_password
```
Copy the entire command output line and past it into the dynamic.yml file.

## Local development environment
This version of the project allows using Traefik by supporting secure connection in a local development environment. Let's encrypt, usually used by Traefik to benefit from an SSL connection, cannot be used in a local environment (localhost). You must therefore generate a self-signed certificate using a utility like mkcert for example:

```
    mkcert -install
```
Then create a directory to store the certificates and generate the certificates for the domains and subdomains that will be used by the project:

```
mkdir certs
mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "project-ci.localhost" "*.project-ci.localhost"
```

## Launch the tools
```
    docker-compose up
```