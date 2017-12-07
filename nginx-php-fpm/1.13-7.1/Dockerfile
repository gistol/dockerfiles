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
    && apk add --no-cache runit \
    && mkdir -p /etc/runit \
    \
    && addgroup -g ${APP_GROUP_ID} -S ${APP_GROUP} \
    && adduser -D -S -h ${APP_USER_HOME} -s ${APP_USER_SHELL} -u ${APP_USER_ID} -G ${APP_GROUP} ${APP_USER}

RUN set -xe \
    \
    && apk add --no-cache nginx

COPY rootfs /

ENTRYPOINT [ "/sbin/entrypoint" ]
CMD [ "app:start" ]
