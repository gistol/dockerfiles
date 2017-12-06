FROM alpine:3.6

### Базовые вещи ###############################################################

# Параментры пользователя от имени которого будем запускать приложение
ENV APP_USER=owox \
    APP_USER_ID=1000 \
    APP_GROUP=owox \
    APP_GROUP_ID=1000 \
    APP_USER_HOME=/home/owox \
    APP_USER_SHELL=/sbin/nologin

RUN set -xe \
    \
    && apk add --no-cache \
        bash \
        ca-certificates \
        curl \
        jq \
        openssl \
        su-exec \
        tar \
    \
    && S6_OVERLAY_VERSION=1.21.2.1 \
    && curl -sSfL https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
        | tar xzf - -C / \
    \
    && addgroup -g ${APP_GROUP_ID} -S ${APP_GROUP} \
    && adduser -D -S -h ${APP_USER_HOME} -s ${APP_USER_SHELL} -u ${APP_USER_ID} -G ${APP_GROUP} ${APP_USER}

RUN set -xe \
    \
    && apk add --no-cache nginx

COPY rootfs /

ENTRYPOINT [ "/sbin/entrypoint" ]
CMD [ "app:start" ]
