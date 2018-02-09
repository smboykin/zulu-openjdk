ARG BASE_IMAGE=ihlchip/alpine-glibc
ARG REPOSITORY
ARG BUILD_VERSION=latest

FROM ${REPOSITORY}${BASE_IMAGE}:${BUILD_VERSION}

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV ZULU_JAVA_VERSION zulu8.25.0.1-jdk8.0.152
ENV ZULU_ARCH ${ZULU_JAVA_VERSION}-linux_x64.tar.gz
ENV JAVA_HOME /usr/lib/jvm/${ZULU_JAVA_VERSION}-linux_x64
ENV JAVA_VERSION 8.152
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

#RUN wget http://cdn.azul.com/zcek/bin/ZuluJCEPolicies.zip \
#    && echo "8021a28b8cac41b44f1421fd210a0a0822fcaf88d62d2e70a35b2ff628a8675a  ZuluJCEPolicies.zip" | sha256sum -c - \
#    && unzip -o ZuluJCEPolicies.zip ZuluJCEPolicies/local_policy.jar -d /tmp/ \
#    && mv /tmp/ZuluJCEPolicies/local_policy.jar ${JAVA_HOME}/jre/lib/security/ \
#    && unzip -o ZuluJCEPolicies.zip ZuluJCEPolicies/US_export_policy.jar -d /tmp/ \
#    && mv /tmp/ZuluJCEPolicies/US_export_policy.jar ${JAVA_HOME}/jre/lib/security/ \
#    && rm -f ZuluJCEPolicies.zip && rm -rf /tmp/ZuluJCEPolicies/ \
#    && chmod -R 640 ${JAVA_HOME}/jre/lib/security/ \
#    && chown -R root:root ${JAVA_HOME}/jre/lib/security/