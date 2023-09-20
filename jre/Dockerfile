FROM alpine:3 AS builder

LABEL maintainer="njmittet@gmail.com"

ARG JRE_URL=https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.8.1+1/
ARG	TGZ_FILE=OpenJDK17U-jre_x64_alpine-linux_hotspot_17.0.8.1_1.tar.gz

RUN apk add --update wget && \
	cd /tmp && \
	wget ${JRE_URL}${TGZ_FILE} -O ${TGZ_FILE} && \
	tar -xf ${TGZ_FILE} -C /opt && \
	rm -f /tmp/* rm /var/cache/apk/*

FROM alpine:3

ARG JRE_DIR="jdk-17.0.8.1+1-jre"

ENV PATH="/opt/${JRE_DIR}/bin:${PATH}"

COPY --from=builder /opt/${JRE_DIR}/ /opt/${JRE_DIR}/

CMD ["java", "--version"]