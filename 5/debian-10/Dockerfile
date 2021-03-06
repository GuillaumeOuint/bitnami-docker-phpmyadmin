FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libaudit1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcurl4 libexpat1 libffi6 libfftw3-double3 libfontconfig1 libfreetype6 libgcc1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmemcached11 libmemcachedutil2 libncurses6 libnettle6 libnghttp2-14 libonig5 libp11-kit0 libpam0g libpcre3 libpng16-16 libpq5 libpsl5 libreadline7 librtmp1 libsasl2-2 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "php" "7.4.11-2" --checksum aabc290a13f5bee0a31bd9edf504fc025d51a7b95abeff9edcef3cf4d90e3393
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "apache" "2.4.46-2" --checksum 8d591562d804902c986f59f18bbea5e47289f4cbfb364c13862d4b9df557e0f4
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mysql-client" "10.3.25-0" --checksum 12528e33966094535a302d557eca3eb1ae7d5c7b8fff4b9e70d96cafedbb5341
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "libphp" "7.4.11-1" --checksum 2a552ec9133bfeea17ee4fa40976499165cc3c6c89e00d483063df083abe4b13
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.0-2" --checksum f1bee333423f6627d795ea9e0b6a89c18d2e5c2e2d015bb4620f040f1d520cf6
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "phpmyadmin" "5.0.3-0" --checksum 221772dff600635c74e2c82e837a0e299d0da1a1bd4b5cbbe8e24c7aafe92982
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.12.0-2" --checksum 4d858ac600c38af8de454c27b7f65c0074ec3069880cb16d259a6e40a46bbc50
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/apache/postunpack.sh
RUN /opt/bitnami/scripts/php/postunpack.sh
RUN /opt/bitnami/scripts/apache-modphp/postunpack.sh
RUN /opt/bitnami/scripts/phpmyadmin/postunpack.sh
ENV ALLOW_EMPTY_PASSWORD="no" \
    APACHE_ENABLE_CUSTOM_PORTS="no" \
    APACHE_HTTPS_PORT_NUMBER="" \
    APACHE_HTTP_PORT_NUMBER="" \
    BITNAMI_APP_NAME="phpmyadmin" \
    BITNAMI_IMAGE_VERSION="5.0.3-debian-10-r3" \
    MARIADB_HOST="mariadb" \
    MARIADB_PORT_NUMBER="3306" \
    MARIADB_ROOT_PASSWORD="" \
    MARIADB_ROOT_USER="root" \
    MYSQL_CLIENT_CREATE_DATABASE_NAME="" \
    MYSQL_CLIENT_CREATE_DATABASE_PASSWORD="" \
    MYSQL_CLIENT_CREATE_DATABASE_PRIVILEGES="ALL" \
    MYSQL_CLIENT_CREATE_DATABASE_USER="" \
    MYSQL_CLIENT_ENABLE_SSL="no" \
    MYSQL_CLIENT_SSL_CA_FILE="" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:/opt/bitnami/apache/bin:/opt/bitnami/mysql/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8080 8443

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/phpmyadmin/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/apache/run.sh" ]
