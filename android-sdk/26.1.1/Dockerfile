FROM alpine:3.7

RUN set -x && \
    \
    apk add --no-cache \
        ca-certificates \
        curl \
        bash

ENV GLIBC_VERSION   "2.27-r0"
ENV LANG            "en_US.UTF-8"
RUN URL="https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}" && \
    PUBKEY_URL="https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub" && \
    PUBKEY="/etc/apk/keys/sgerrand.rsa.pub" && \
    \
    set -x && \
    \
    curl -sS -f ${PUBKEY_URL} -o ${PUBKEY} && \
    curl -sSL -f ${URL}/glibc-${GLIBC_VERSION}.apk -o glibc-${GLIBC_VERSION}.apk && \
    curl -sSL -f ${URL}/glibc-bin-${GLIBC_VERSION}.apk -o glibc-bin-${GLIBC_VERSION}.apk && \
    curl -sSL -f ${URL}/glibc-i18n-${GLIBC_VERSION}.apk -o glibc-i18n-${GLIBC_VERSION}.apk && \
    \
    apk add --no-cache glibc-${GLIBC_VERSION}.apk && \
    \
    apk add --no-cache --virtual=.i18n-dependencies \
      glibc-bin-${GLIBC_VERSION}.apk \
      glibc-i18n-${GLIBC_VERSION}.apk && \
    /usr/glibc-compat/bin/localedef -i ${LANG%.*} -f ${LANG#*.} ${LANG} && \
    \
    apk del --no-cache .i18n-dependencies && \
    rm \
        ${PUBKEY} \
        glibc-${GLIBC_VERSION}.apk \
        glibc-bin-${GLIBC_VERSION}.apk \
        glibc-i18n-${GLIBC_VERSION}.apk

ENV JAVA_VERSION          "8u171"
ENV JAVA_ALPINE_VERSION   "8.171.11-r0"
ENV JAVA_HOME             "/usr/lib/jvm/java-1.8-openjdk"
ENV PATH                  "${JAVA_HOME}/jre/bin:$PATH"
RUN set -x && \
    \
    apk add --no-cache \
        openjdk8="$JAVA_ALPINE_VERSION"

ENV SDK_TOOLS       "26.1.1"
ENV BUILD_TOOLS     "28.0.0"
ENV PLATFORM_TOOLS  "28.0.0"
ENV ANDROID_HOME    "/opt/android-sdk"
ENV PATH            "${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/${BUILD_TOOLS}:${ANDROID_HOME}/platform-tools:$PATH"
RUN URL="http://dl.google.com/android/repository/sdk-tools-linux-4333796.zip" && \
    \
    set -x && \
    \
    curl -sSL -f ${URL} -o sdk-tools.zip && \
	  mkdir -p ${ANDROID_HOME} && \
	  unzip sdk-tools.zip -d ${ANDROID_HOME} && \
	  rm sdk-tools.zip && \
    \
    mkdir -p ${HOME}/.android/ && touch ${HOME}/.android/repositories.cfg && \
    yes | ${ANDROID_HOME}/tools/bin/sdkmanager "--licenses" > /dev/null && \
    ${ANDROID_HOME}/tools/bin/sdkmanager "--update" && \
    \
    ${ANDROID_HOME}/tools/bin/sdkmanager \
      "build-tools;${BUILD_TOOLS}" \
      "platform-tools"
