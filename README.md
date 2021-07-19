# alpine-adoptopenjdk

JDK or JRE base image based on AdoptOpenJDK and Alpine Linux.

As this images provides AdoptOpenJDK 16, it's created without having to add glibc, as [JEP 386 landed support for using OpenJDK on Alpine Linux](https://openjdk.java.net/jeps/386).

## Usage

The JRE image is intended to be used as a base image for images running Java applications:

```Dockerfile
FROM njmittet/alpine-adoptopenjdk:16-jre-hotspot

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

The JDK image is ment to be used as the build stage in a multi-stage Docker build:

```Dockerfile
FROM alpine-adoptopenjdk:16-jdk-hotspot AS builder

WORKDIR /tmp

RUN apk add --update git && \
    git clone https://github.com/njmittet/demo-application.git && \
    cd demo-application && \
    ./gradlew build && \
    rm -rf /var/cache/apk/*

FROM alpine-adoptopenjdk:16-jre-hotspot

USER root
ENV JAVA_DIR /opt/java

RUN mkdir -p $JAVA_DIR && \
             adduser -D -h $JAVA_DIR java

USER java
WORKDIR $JAVA_DIR

COPY --from=builder /tmp/demo-application/build/libs/demo-application.jar .

EXPOSE 8080
CMD ["java", "-jar", "demo-application.jar"]
```

## Resources

- [AdoptOpenJDK for Alpine Linux With musl libc](https://blog.adoptopenjdk.net/2021/03/adoptopenjdk-16-available/)
- [Multi-Stage Docker Builds](https://docs.docker.com/develop/develop-images/multistage-build/)
- [AdoptOpenJDK OpenJDK 16](https://adoptopenjdk.net/releases.html?variant=openjdk16&jvmVariant=hotspot)