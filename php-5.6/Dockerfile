FROM tekintian/docker-base:latest
LABEL maintainer="TekinTian tekintian@gmail.com"

ENV PHP_INSTALL_DIR=/usr/local/php \
    PHP_VERSION=5.6.38 \
    IMAGICK_PECL_VERSION=3.4.3 \
    LIBMEMCACHED_VERSION=1.0.18 \
    REDIS_PECL_VERSION=4.2.0 \
    SWOOLE_VERSION=1.9.23 \
    OPENSSL_VERSION=1.0.2o \
    REMOTE_SRC_PATH=https://github.com/tekintian/php/raw/master

WORKDIR /tmp

#install build deps
RUN  buildDeps='\
               gcc \
                g++ \
                make \
                cmake \
                autoconf \
                libssl-dev \
                libcurl4-openssl-dev \
                libxslt-dev \
                libicu-dev \
                libxml2-dev \
                libjpeg-dev \
                libpng-dev \
                libfreetype6-dev \
                libsasl2-dev \
                libevent-dev \
                libpcre3-dev \
                libgpgme11-dev \
                pkg-config \
                patch \
                --no-install-recommends ' \
        && apt-get update && apt-get install -y ${buildDeps} && rm -rf /var/lib/apt/lists/*  && \
#openssl old version install. 
    wget -c --no-check-certificate https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz && \
    tar xzf openssl-${OPENSSL_VERSION}.tar.gz && \
    cd openssl-${OPENSSL_VERSION} && \
    ./config --prefix=/usr/local/openssl_10 && ./config -t && \
    make && make install && \
    ln -s /usr/local/openssl_10 /usr/local/openssl && \
    cd /tmp/ && \
# install php
    wget -c --no-check-certificate http://www.php.net/distributions/php-${PHP_VERSION}.tar.gz && \
    tar xzf php-${PHP_VERSION}.tar.gz && \
    cd php-${PHP_VERSION} && \
    ./buildconf --force && \
    ./configure --prefix=${PHP_INSTALL_DIR} --with-config-file-path=${PHP_INSTALL_DIR}/etc \
        --with-config-file-scan-dir=${PHP_INSTALL_DIR}/etc/php.d \
        --with-fpm-user=$RUN_USER --with-fpm-group=$RUN_USER --enable-fpm --enable-opcache \
        --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
        --with-iconv-dir=/usr/local --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib \
        --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-exif \
        --enable-sysvsem --enable-inline-optimization --with-curl=/usr/local --enable-mbregex \
        --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl=/usr/local/openssl_10 \
        --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-ftp --enable-intl --with-xsl \
        --with-gettext --enable-zip --enable-soap --disable-debug && \
    make ZEND_EXTRA_LIBS='-liconv' && \
    make install && \
    mkdir -p ${PHP_INSTALL_DIR}/etc/php.d && \
    /bin/cp php.ini-production ${PHP_INSTALL_DIR}/etc/php.ini && \
    /bin/cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && \
    chmod +x /etc/init.d/php-fpm && \
   cd /tmp/ && \
# install ImageMagick
    wget -c --no-check-certificate http://www.imagemagick.org/download/ImageMagick.tar.gz && \
    tar xzf ImageMagick.tar.gz && \
    cd ImageMagick* && \
    ./configure --prefix=/usr/local/imagemagick --enable-shared --enable-static && \
    make && make install && \
    cd /tmp/ && \
    wget -c --no-check-certificate http://pecl.php.net/get/imagick-${IMAGICK_PECL_VERSION}.tgz && \
    tar xzf imagick-${IMAGICK_PECL_VERSION}.tgz && \
    cd imagick-${IMAGICK_PECL_VERSION} && \
    export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config --with-imagick=/usr/local/imagemagick && \
    make && make install && \
    echo "extension=imagick.so" > ${PHP_INSTALL_DIR}/etc/php.d/ext-imagick.ini && \
    cd /tmp/ && \
# install php-memcache
# git clone https://github.com/websupport-sk/pecl-memcache.git
# cd pecl-memcache
    wget -c --no-check-certificate http://mirrors.linuxeye.com/oneinstack/src/memcache-2.2.7.tgz && \
    tar xzf memcache-2.2.7.tgz && \
    cd memcache-2.2.7 && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config && \
    make && make install && \
    echo "extension=memcache.so" > ${PHP_INSTALL_DIR}/etc/php.d/ext-memcache.ini && \
    cd /tmp/ && \
# install php-memcached
# git clone -b php7 https://github.com/php-memcached-dev/php-memcached.git
# cd php-memcached
    wget -c --no-check-certificate https://launchpad.net/libmemcached/1.0/${LIBMEMCACHED_VERSION}/+download/libmemcached-${LIBMEMCACHED_VERSION}.tar.gz && \
    tar xzf libmemcached-${LIBMEMCACHED_VERSION}.tar.gz && \
    cd libmemcached-${LIBMEMCACHED_VERSION} && \
    sed -i "s@lthread -pthread -pthreads@lthread -lpthread -pthreads@" ./configure && \
    ./configure && \
    make && make install && \
    cd /tmp/ && \
    wget -c --no-check-certificate http://mirrors.linuxeye.com/oneinstack/src/memcached-2.2.0.tgz && \
    tar xzf memcached-2.2.0.tgz && \
    cd memcached-2.2.0 && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config && \
    make && make install && \
    echo "extension=memcached.so" >> ${PHP_INSTALL_DIR}/etc/php.d/ext-memcached.ini && \
    echo "memcached.use_sasl=1" >> ${PHP_INSTALL_DIR}/etc/php.d/ext-memcached.ini && \
    cd /tmp/ && \
# install php-redis
    wget -c --no-check-certificate http://pecl.php.net/get/redis-${REDIS_PECL_VERSION}.tgz && \
    tar xzf redis-${REDIS_PECL_VERSION}.tgz && \
    cd redis-${REDIS_PECL_VERSION} && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config && \
    make && make install && \
    echo "extension=redis.so" > ${PHP_INSTALL_DIR}/etc/php.d/ext-redis.ini && \
    cd /tmp/ && \

# install swoole
    wget -c --no-check-certificate http://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz  && \
    tar xzf swoole-${SWOOLE_VERSION}.tgz && \
    cd swoole-${SWOOLE_VERSION} && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config --enable-jemalloc && \
    make && make install && \
    echo "extension=swoole.so" > ${PHP_INSTALL_DIR}/etc/php.d/ext-swoole.ini && \
    
# install composer
    wget -c --no-check-certificate https://getcomposer.org/composer.phar && \
    mv composer.phar /usr/local/bin/composer && \
    chmod a+x /usr/local/bin/composer && \
    cd /tmp/ && rm -rf /tmp/* && \
#remove build deps
    apt-get purge -y --auto-remove ${buildDeps}

ADD ${REMOTE_SRC_PATH}/ext-opcache.ini ${PHP_INSTALL_DIR}/etc/php.d/ext-opcache.ini

# run install script
ADD ${REMOTE_SRC_PATH}/install_zend.sh /tmp/install_zend.sh
RUN chmod 777 install_zend.sh && \
    bash install_zend.sh && \
    rm -rf /tmp/*

# Set the entrypoint script.
ADD ${REMOTE_SRC_PATH}/entrypoint.sh /entrypoint.sh
RUN chmod 777 /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# Define the default command.
CMD ["bash"]