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


# build
Build a snapshot from a clean working directory
```bash
$ ./gradlew releaseCheck clean build -Prelease.stage=SNAPSHOT -Prelease.scope=patch
```

When a parameter `minimal` is provided, certain tasks will be skipped to make the build faster.
e.g. `findbugs`, `checkstyle`, `javadoc` - tasks which results are not essential for a working build.
```bash
./gradlew clean build -Pminimal
```

## publish SNAPSHOT to local maven repository
```
./gradlew clean build -Pminimal -Prelease.stage=SNAPSHOT -Prelease.scope=patch publishToMavenLocal
```

## create a release
```bash
./gradlew final -Prelease.scope=patch
```

# license
The project is licensed under the Apache License. See [LICENSE](LICENSE) for details.

