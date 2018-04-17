ARG BASE_IMAGE=ihlchip/alpine-glibc
ARG REPOSITORY
ARG BUILD_VERSION=latest

FROM ${REPOSITORY}${BASE_IMAGE}:${BUILD_VERSION}

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV ZULU_JAVA_VERSION zulu8.27.0.7-jdk8.0.162
ENV ZULU_ARCH ${ZULU_JAVA_VERSION}-linux_x64.tar.gz
ENV JAVA_HOME /usr/lib/jvm/${ZULU_JAVA_VERSION}-linux_x64
ENV JAVA_VERSION 8.162
RUN INSTALL_DIR=/usr/lib/jvm && \
	BIN_DIR=/usr/bin && \
	MAN_DIR=/usr/share/man/man1 && \
	ZULU_DIR=$( basename ${ZULU_ARCH} .tar.gz ) && \
	apk update && \
	apk add --no-cache ca-certificates wget && \
	update-ca-certificates && \
	wget -q http://cdn.azul.com/zulu/bin/${ZULU_ARCH} && \
	mkdir -p ${INSTALL_DIR} && \
	tar -xf ./${ZULU_ARCH} -C /usr/lib/jvm/ && rm -f ${ZULU_ARCH} && \
	cd ${BIN_DIR}; find ${INSTALL_DIR}/${ZULU_DIR}/bin -type f -perm -a=x -exec ln -s {} . \; && \
	mkdir -p ${MAN_DIR} && \
	cd ${MAN_DIR}; find ${INSTALL_DIR}/${ZULU_DIR}/man/man1 -type f -name "*.1" -exec ln -s {} . \; && \
	java -version 

# Set JDK security cryptography to 'unlimited' for strong encryption
RUN cp $JAVA_HOME/jre/lib/security/policy/unlimited/*.jar $JAVA_HOME/jre/lib/security/
