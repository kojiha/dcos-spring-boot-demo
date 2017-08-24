# Simple Spring Boot project for DC/OS marathon deployment.

## Features
- Spring Boot startup project with 2 web entry points.
- Dockerfile is created as instructed here: https://spring.io/guides/gs/spring-boot-docker/
- marathon.json for DC/OS deployment
- Jenkinsfile for CI/CD pipeline


## Project specific values
- Marathon-LB Group: external
- Virtual host: spring-boot-demo.app.dcos.local
- Docker registry: gitlab.app.dcos.local:50000
- Image tag: gitlab.app.dcos.local:50000/dcos/spring-boot-demo:latest
- Jenkins credential ID to authenticate with GitLab: gitlab

## Entry points
- http://spring-boot-demo.app.dcos.local/
- http://spring-boot-demo.app.dcos.local/about


