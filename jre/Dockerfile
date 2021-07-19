FROM alpine:3

LABEL maintainer="njmittet@gmail.com"

ENV JRE_URL=https://github.com/AdoptOpenJDK/openjdk16-binaries/releases/download/jdk-16.0.1+9/ \
	TGZ_FILE=OpenJDK16U-jre_x64_alpine-linux_hotspot_16.0.1_9.tar.gz \
	PATH="/opt/jdk-16.0.1+9-jre/bin:${PATH}"

RUN apk add --update wget && \
	cd /tmp && \
	wget ${JRE_URL}${TGZ_FILE} -O ${TGZ_FILE} && \
	tar -xf ${TGZ_FILE} -C /opt && \
	rm -f /tmp/* rm /var/cache/apk/*

CMD ["java", "--version"]