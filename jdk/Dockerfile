FROM alpine:3 AS builder

LABEL maintainer="njmittet@gmail.com"

ENV JRE_URL=https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.8.1+1/ \
	TGZ_FILE=OpenJDK17U-jdk_x64_alpine-linux_hotspot_17.0.8.1_1.tar.gz

RUN apk add --update wget && \
	cd /tmp && \
	wget ${JRE_URL}${TGZ_FILE} -O ${TGZ_FILE} && \
	tar -xf ${TGZ_FILE} -C /opt && \
	rm -f /tmp/* rm /var/cache/apk/*

FROM alpine:3

ARG JDK_DIR="jdk-17.0.8.1+1"

ENV PATH="/opt/${JDK_DIR}/bin:${PATH}"

COPY --from=builder /opt/${JDK_DIR}/ /opt/${JDK_DIR}/

CMD ["javac", "--version"]