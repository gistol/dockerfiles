FROM alpine:3.6

RUN set -xe ;\

    apk add --no-cache --virtual .fetch-dependencies \
      curl \
    ;\

    S6_OVERLAY_VERSION="1.20.0.0" ;\
    S6_OVERLAY_SHA256="8827420ce1bcdb5f6219e3295c4b7cb46b7829ce0156ebc5310d782215bbcde1" ;\
    S6_OVERLAY_URL="https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz" ;\
    S6_OVERLAY_TAR="/tmp/s6.tar.gz" ;\
    curl --retry 7 -Lsfo ${S6_OVERLAY_TAR} ${S6_OVERLAY_URL} ;\
    echo -n "${S6_OVERLAY_SHA256}  ${S6_OVERLAY_TAR}" | sha256sum -c > /dev/null ;\
    tar xzf ${S6_OVERLAY_TAR} -C / ;\
    rm -f ${S6_OVERLAY_TAR} ;\
    mkdir -p /etc/{fix-attrs.d,cont-init.d,services.d,cont-finish.d} ;\

    apk del .fetch-dependencies

RUN set -xe ;\

    apk add --no-cache \
      expat \
      libpq \
      libstdc++ \
      mariadb-client-libs \
      unixodbc

RUN set -xe ;\

    SPHINX_GROUP=sphinx ;\
    SPHINX_USER=sphinx ;\
    addgroup -g 82 -S ${SPHINX_GROUP} ;\
    adduser -u 82 -S -D -h /var/lib/sphinx -s /sbin/nologin -G ${SPHINX_GROUP} ${SPHINX_USER} ;\
    mkdir -p /var/log/sphinx /var/run/sphinx ;\
    chown ${SPHINX_USER}:${SPHINX_GROUP} /var/log/sphinx /var/run/sphinx ;\

    apk add --no-cache --virtual .sphinx-build-dependencies \
      curl \
      expat-dev \
      g++ \
      gcc \
      libre2-dev \
      make \
      mariadb-dev \
      postgresql-dev \
      snowball-dev \
      unixodbc-dev \
    ;\

    SPHINX_VERSION=2.3.2-beta ;\
    SPHINX_SOURCE="http://sphinxsearch.com/files/sphinx-${SPHINX_VERSION}.tar.gz" ;\
    curl -fSL --connect-timeout 30 ${SPHINX_SOURCE} | tar xz -C /tmp ;\
    cd /tmp/sphinx-${SPHINX_VERSION} ;\

    sed -i 's/PAGE_SIZE/PAGE_SIZES/g' src/sphinx.cpp ;\
    sed -i '1493s/#ifndef __APPLE__/#if defined(linux) \&\& defined(__GLIBC__)/' src/sphinxstd.cpp ;\

    ./configure \
      --prefix=/usr \
      --exec-prefix=/usr \
      --sysconfdir=/etc/sphinx \
      --localstatedir=/var/lib/sphinx \
      --enable-id64 \
      --with-iconv \
      --with-libexpat \
      --with-libstemmer \
      --with-mysql \
      --with-pgsql \
      --with-re2 \
      --with-unixodbc \
    ;\
    make -j $(getconf _NPROCESSORS_ONLN) ;\
    make install ;\

    apk del .sphinx-build-dependencies ;\
    rm -rf /tmp/sphinx-${SPHINX_VERSION}

CMD ["/init"]
