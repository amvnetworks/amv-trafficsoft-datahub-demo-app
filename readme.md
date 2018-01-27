[![Build Status](https://travis-ci.org/amvnetworks/amv-trafficsoft-datahub-demo-app.svg?branch=master)](https://travis-ci.org/amvnetworks/amv-trafficsoft-datahub-demo-app)
[![License](https://img.shields.io/github/license/amvnetworks/amv-trafficsoft-datahub-demo-app.svg?maxAge=2592000)](https://github.com/amvnetworks/amv-trafficsoft-datahub-demo-app/blob/master/LICENSE)

amv-trafficsoft-datahub-demo-app
========

amv-trafficsoft-datahub-demo-app is a showcase application using [amv-trafficsoft-datahub](https://github.com/amvnetworks/amv-trafficsoft-datahub).
This software is currently only used for demo purposes and should not be used in a production environment.

# usage
The [application.yml](example-app/src/main/resources/application.yml) acts as a
template for your own configuration parameter.

## configuration
Copy the contents of the `application.yml` file to `application-my-profile.yml`
and start the application with `--spring.profiles.active=my-profile`.
Or simply adapt the `application.yml` contents to your needs.

A typical configuration can look something like this:
```yml
spring.application.name: 'amv-trafficsoft-datahub-demo-app'
spring.profiles.active: demo

logging.config: classpath:logback-spring.xml

server.port: 9000

amv.trafficsoft.api.rest:
  baseUrl: 'https://www.example.com'
  username: 'john_doe'
  password: 'mysupersecretpassword'
  contractId: 0

amv.trafficsoft.datahub.xfcd:
  enabled: true
  fetch-interval-in-seconds: 120
  initial-fetch-delay-in-seconds: 30
  max-amount-of-nodes-per-delivery: 5000
  refetch-immediately-on-delivery-with-max-amount-of-nodes: true

amv.trafficsoft.xfcd.consumer.jdbc:
  enabled: true
  jdbcUrl: 'jdbc:sqlite:trafficsoft-datahub-example-app.db'
  driverClassName: 'org.sqlite.JDBC'
  schemaMigrationEnabled: true
  flywayScriptsLocation: 'db/sqlite/xfcd/migration'
  sendConfirmationEvents: true
  pool:
    max-pool-size: 1

```
An application loaded with these configuration values would:
  - start a webserver on port 9000
  - use a sqlite database named 'trafficsoft-datahub-example-app.db' in the working directory
  - wait and initial 30s before retrieving data every 120s 

## build & run
### gradle + sprint-boot
Run the application with active `development` profile
```bash
$ ./gradlew example-app:bootRun -Dspring.profiles.active=development
```

### jar file
Building and running the final jar
```bash
$ ./gradlew clean build -Pminimal && java -jar example-app/build/libs/example-app-<version>.jar
 --spring.profiles.active=production,debug
```
Check application is up and running
```bash
$ curl localhost:9001/manage/health
{
  "status" : "UP",
  "diskSpace" : {
    "status" : "UP",
    "total" : 511740735488,
    "free" : 277131235328,
    "threshold" : 10485760
  },
  "db" : {
    "status" : "UP",
    "database" : "MySQL",
    "hello" : 1
  }
}
```

## development
### build
Build a snapshot from a clean working directory
```bash
$ ./gradlew releaseCheck clean build -Prelease.stage=SNAPSHOT -Prelease.scope=patch
```

When a parameter `minimal` is provided, certain tasks will be skipped to make the build faster.
e.g. `findbugs`, `checkstyle`, `javadoc` - tasks which results are not essential for a working build.
```bash
./gradlew clean build -Pminimal
```

### publish SNAPSHOT to local maven repository
```
./gradlew clean build -Pminimal -Prelease.stage=SNAPSHOT -Prelease.scope=patch publishToMavenLocal
```

### create a release
```bash
./gradlew final -Prelease.scope=patch
```

### IDE
As this project uses [Project Lombok](https://projectlombok.org/) make sure you have annotation processing enabled.

### metrics
Prometheus is supported as monitoring system. Metrics can be pulled from `http://localhost:9001/manage/prometheus` 


# license
The project is licensed under the Apache License. See [LICENSE](LICENSE) for details.

