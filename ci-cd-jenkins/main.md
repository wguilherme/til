## oi

- sonar
- quality gate
- jenkins


## Ambiente
### Setup
- Git
- Java
- Jenkins
- Maven (empacotamento)
- Eclipse
- Tomcat (deploy)
- Docker
- PostgreSQL
- SonarQube
- Selenium
- Docker compose

#


- Tomcat
  - brew services start tomcat
  - brew services stop tomcat
  - brew services restart tomcat
  - brew services status tomcat
  <!-- discover tomcat location -->
  - brew ls tomcat

Change config
filepath: conf/tomcat-users.xml

<!-- my location macos bigsur -->
/usr/local/Cellar/tomcat/10.0.21/libexec/conf/tomcat-users.xml

```
<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
```
manager-gui
manager-script
manager-jmx
manager-status



Git diff lista as mudanças no arquivo


## Banco de dados


basic postgres 9.6 docker command line

```
docker run --name pg-tasks -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=tasks -p 5433:5432 postgres:9.6

```

Basic postgres 9.6 docker-compose

```
version: "3"

services:
  pg-tasks:
    container_name: pg-tasks
    image: postgres:9.6
    ports: 
      - "5433:5432"
    environment:
      - POSTGRES_DB=tasks
      # - POSTGRES_USER=tasks_user
      - POSTGRES_PASSWORD=password
```