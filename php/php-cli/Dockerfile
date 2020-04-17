ARG PHP_VERSION="7.4"
ARG COMPOSER_VERSION="1.9.3"
ARG DEBIAN_TAG="stable-20200224-slim"

FROM composer:${COMPOSER_VERSION} AS composer

FROM debian:${DEBIAN_TAG}

ENV DEBIAN_FRONTEND noninteractive

# Install minimal system requirements
RUN true \
 && apt-get -qq update \
 && apt-get -qy install --no-install-recommends \
        ca-certificates \
        wget \
 && rm -rf /var/lib/apt/lists/* \
;

# Install minimal PHP composer dependencies
ARG PHP_VERSION
RUN true \
 && wget -qq -O /etc/apt/trusted.gpg.d/php.gpg "https://packages.sury.org/php/apt.gpg" \
 && echo "deb https://packages.sury.org/php/ $(. /etc/os-release && echo $VERSION_CODENAME) main" > /etc/apt/sources.list.d/php.list \
 && apt-get -qq update \
 && apt-get -qy install --no-install-recommends \
        "php${PHP_VERSION}-bcmath" \
        "php${PHP_VERSION}-cli" \
        "php${PHP_VERSION}-curl" \
        "php${PHP_VERSION}-json" \
        "php${PHP_VERSION}-mbstring" \
        "php${PHP_VERSION}-xml" \
        "php${PHP_VERSION}-zip" \
 && rm -rf /var/lib/apt/lists/* \
;

# Install developer tools
ARG HOST_IP
RUN true \
 && apt-get -qq update \
 && apt-get -qy install --no-install-recommends \
        "php${PHP_VERSION}-xdebug" \
        unzip \
        vim \
 && phpdismod xdebug \
 && rm -rf /var/lib/apt/lists/* \
;
COPY etc/xdebug.ini "/etc/php/${PHP_VERSION}/mods-available/xdebug.ini"

# Add custom user
ARG GROUP_ID=1000
ARG USER_ID=1000
RUN true \
 && addgroup user --gid "${GROUP_ID}" \
 && useradd user --uid "${USER_ID}" --gid user -d /home/user -s /bin/bash \
 && mkdir -p /home/user \
 && chown -R user:user /home/user \
;

USER user

# Install Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN true \
 && composer global require hirak/prestissimo \
 && composer global clear-cache --quiet \
;

WORKDIR /opt

ENTRYPOINT ["/bin/bash"]
