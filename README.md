# alpine-temurin

A JDK or JRE base image based on [Eclipse Temurin](https://projects.eclipse.org/projects/adoptium.temurin)
and [Alpine Linux](https://www.alpinelinux.org/).

See the [image on Docker Hub](https://hub.docker.com/repository/docker/njmittet/alpine-temurin).

## Usage

The JRE image is intended to be used as a base image for running Java application images:

```Dockerfile
FROM njmittet/alpine-temurin:17-jre

USER root
ENV JAVA_DIR /opt/java

RUN mkdir -p $JAVA_DIR && \
    adduser -D -h $JAVA_DIR java

USER java
WORKDIR $JAVA_DIR
COPY application.jar $JAVA_DIR

EXPOSE 9000
CMD ["java", "-jar", "application.jar"]
```

The JDK image is intended to be used in the build stage of a multi-stage Docker build:

```Dockerfile
FROM njmittet/alpine-temurin:17-jdk AS builder

WORKDIR /tmp

RUN apk add --update git && \
    git clone https://github.com/njmittet/demo-application.git && \
    cd demo-application && \
    ./gradlew build

FROM njmittet/alpine-temurin:17-jre

USER root
ENV JAVA_DIR /opt/java

RUN mkdir -p $JAVA_DIR && \
             adduser -D -h $JAVA_DIR java

USER java
WORKDIR $JAVA_DIR

COPY --from=builder /tmp/demo-application/build/libs/application.jar .

EXPOSE 8080
CMD ["java", "-jar", "application.jar"]
```
