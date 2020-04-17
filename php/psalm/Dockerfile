ARG ALPINE_VERSION=3.9
FROM alpine:${ALPINE_VERSION}

ADD https://repos.php.earth/alpine/phpearth.rsa.pub /etc/apk/keys/phpearth.rsa.pub
ARG ALPINE_VERSION
ARG PHP_VERSION=7.4
RUN true \
    && echo "https://repos.php.earth/alpine/v${ALPINE_VERSION}" >> /etc/apk/repositories \
    && apk add --no-cache \
            php${PHP_VERSION} \
            php${PHP_VERSION}-dom \
            php${PHP_VERSION}-json \
            php${PHP_VERSION}-opcache \
            php${PHP_VERSION}-phar \
            php${PHP_VERSION}-simplexml \
            php${PHP_VERSION}-tokenizer \
            tini \
    && rm -rf /var/cache/apk/* \
;

ARG PSALM_VERSION
ADD https://github.com/vimeo/psalm/releases/download/${PSALM_VERSION}/psalm.phar /usr/local/bin/psalm
RUN chmod +x /usr/local/bin/psalm

ENTRYPOINT ["tini", "--", "psalm"]
WORKDIR /code
