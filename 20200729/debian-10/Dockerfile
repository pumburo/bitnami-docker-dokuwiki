FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl sudo ca-certificates curl gzip libargon2-1 libaudit1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcurl4 libexpat1 libffi6 libfftw3-double3 libfontconfig1 libfreetype6 libgcc1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmemcached11 libmemcachedutil2 libncurses6 libnettle6 libnghttp2-14 libp11-kit0 libpam0g libpcre3 libpng16-16 libpq5 libpsl5 libreadline7 librtmp1 libsasl2-2 libsodium23 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "php" "7.3.27-5" --checksum 26a89b152b8e73d5301a7d5b136dcfe023a46743e892edfb94a6d34ba83f4726
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "apache" "2.4.46-5" --checksum f6663ef2cd47d80d63af1da7ff319ed94afe8a825115b2f52894d799013b551c
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "libphp" "7.3.27-2" --checksum 136766df799e2acb375a4b2b46eb8b7e27899287f8dd22bfd8f205d6bea3c59d
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.0-3" --checksum 8179ad1371c9a7d897fe3b1bf53bbe763f94edafef19acad2498dd48b3674efe
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.12.0-2" --checksum 4d858ac600c38af8de454c27b7f65c0074ec3069880cb16d259a6e40a46bbc50
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "dokuwiki" "20200729.0.0-1" --checksum d2f0229f0f6c663aeae3f77b297f8c415aae81543a2206686387a0b622411086
RUN chmod g+rwX /opt/bitnami
RUN useradd -m -p $(perl -e 'print crypt($ARGV[0], "password")' '1') -s /bin/bash wikiadm
RUN usermod -aG sudo wikiadm

COPY rootfs /
RUN /opt/bitnami/scripts/apache/postunpack.sh
RUN /opt/bitnami/scripts/php/postunpack.sh
RUN /opt/bitnami/scripts/apache-modphp/postunpack.sh
RUN /opt/bitnami/scripts/dokuwiki/postunpack.sh
ENV APACHE_ENABLE_CUSTOM_PORTS="no" \
    APACHE_HTTPS_PORT_NUMBER="" \
    APACHE_HTTP_PORT_NUMBER="" \
    BITNAMI_APP_NAME="dokuwiki" \
    BITNAMI_IMAGE_VERSION="20200729.0.0-debian-10-r196" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:/opt/bitnami/apache/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8080 8443
USER wikiadm
# RUN usermod -aG sudo $( cat /etc/passwd | tail -1 | awk '{ print $1 }' | head -c 10)
ENTRYPOINT [ "/opt/bitnami/scripts/dokuwiki/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/apache/run.sh" ]
