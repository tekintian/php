FROM tekintian/docker-base:1.2
MAINTAINER Tekin Tian <tekintian@gmail.com>

ENV PHP_INSTALL_DIR=/usr/local/php \
    PHP_VERSION=5.3.29 \
    IMAGICK_PECL_VERSION=3.3.0 \
    LIBMEMCACHED_VERSION=1.0.18 \
    REDIS_PECL_VERSION=3.1.0 \
    SWOOLE_VERSION=1.9.3 \
    OPENSSL_VERSION=1.0.2o \
    REMOTE_SRC_PATH=https://github.com/tekintian/php/raw/master

WORKDIR /tmp
ADD src/ /tmp
# install php
RUN buildDeps='\
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
  # wget -c --no-check-certificate https://github.com/openssl/openssl/archive/OpenSSL_1_0_0-stable.tar.gz -O /tmp/openssl-OpenSSL_1_0_0-stable.tar.gz && \
    cd /tmp/ && \
    tar xzf openssl-OpenSSL_1_0_0-stable.tar.gz && \
    cd openssl-OpenSSL_1_0_0-stable && \
    ./config --prefix=/usr/local/openssl && ./config -t && \
    make && make install && \
    cd /tmp/ && \
  #  wget -c --no-check-certificate http://downloads.sourceforge.net/project/re2c/0.16/re2c-0.16.tar.gz && \
    tar xzf re2c-0.16.tar.gz && \
    cd re2c-0.16 && \
    ./configure && make && make install && \
   cd /tmp/  && \
  #  wget -c --no-check-certificate http://ftp.gnu.org/gnu/autoconf/autoconf-2.13.tar.gz && \
    tar xzf autoconf-2.13.tar.gz && \
    cd autoconf-2.13 && \
    ./configure --prefix=/usr/local/autoconf-2.13 && \
    make && make install && \
    export PHP_AUTOCONF=/usr/local/autoconf-2.13/bin/autoconf && \
    export PHP_AUTOHEADER=/usr/local/autoconf-2.13/bin/autoheader  && \
    cd /tmp/  && \
   #  wget -c --no-check-certificate http://www.php.net/distributions/php-${PHP_VERSION}.tar.gz && \
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
        --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl=/usr/local/openssl \
        --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-ftp --enable-intl --with-xsl \
        --with-gettext --enable-zip --enable-soap --disable-debug && \
    sed -i '/^BUILD_/ s/\$(CC)/\$(CXX)/g' Makefile && \
    make ZEND_EXTRA_LIBS='-liconv' &&  \
    make install && \
    mkdir -p ${PHP_INSTALL_DIR}/etc/php.d && \
    /bin/cp php.ini-production ${PHP_INSTALL_DIR}/etc/php.ini && \
    /bin/cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && \
    chmod +x /etc/init.d/php-fpm && \
# cd php-memcached https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz
	cd /tmp/ && \
   # wget -c --no-check-certificate https://launchpad.net/libmemcached/1.0/${LIBMEMCACHED_VERSION}/+download/libmemcached-${LIBMEMCACHED_VERSION}.tar.gz && \
    tar xzf libmemcached-1.0.18.tar.gz && \
    cd libmemcached-1.0.18 && \
    sed -i "s@lthread -pthread -pthreads@lthread -lpthread -pthreads@" ./configure && \
    ./configure && \
    make && make install && \
    cd /tmp/ && \
    #wget -c --no-check-certificate http://mirrors.linuxeye.com/oneinstack/src/memcached-2.2.0.tgz && \
    tar -zxf memcached-2.2.0.tgz && \
    cd memcached-2.2.0 && \
    ${PHP_INSTALL_DIR}/bin/phpize && \
    ./configure --with-php-config=${PHP_INSTALL_DIR}/bin/php-config && \
    make && make install && \
    echo "extension=memcached.so" >> ${PHP_INSTALL_DIR}/etc/php.d/ext-memcached.ini && \
    echo "memcached.use_sasl=1" >> ${PHP_INSTALL_DIR}/etc/php.d/ext-memcached.ini && \
    cd /tmp/ && rm -rf /tmp/* && \
#remove build deps
    apt-get purge -y --auto-remove ${buildDeps}

ADD ${REMOTE_SRC_PATH}/ext-opcache.ini ${PHP_INSTALL_DIR}/etc/php.d/ext-opcache.ini

# run install script
ADD ${REMOTE_SRC_PATH}/install.sh /tmp/install.sh
RUN chmod 777 install.sh && \
    bash install.sh && \
    rm -rf /tmp/*

# install composer
RUN curl -sS https://getcomposer.org/installer | ${PHP_INSTALL_DIR}/bin/php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod a+x /usr/local/bin/composer

# Set the entrypoint script.
ADD ${REMOTE_SRC_PATH}/entrypoint.sh /entrypoint.sh
RUN chmod 777 /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# Define the default command.
CMD ["bash"]