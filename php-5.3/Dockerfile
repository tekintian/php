FROM tekintian/base:latest
MAINTAINER Tekin Tian <tekintian@gmail.com>

ENV PHP_INSTALL_DIR=/usr/local/php \
    PHP_VERSION=5.3.29 \
    IMAGICK_PECL_VERSION=3.3.0 \
    LIBMEMCACHED_VERSION=1.0.18 \
    REDIS_PECL_VERSION=3.1.0 \
    SWOOLE_VERSION=1.9.3 \
    REMOTE_PATH=https://github.com/tekintian/docker-php/raw/master

WORKDIR /tmp

# install php
RUN apt-get remove autoconf -y && \
    apt-get autoremove -y && \
   apt-get install -y libfreetype6-dev && \
   mkdir /usr/include/freetype2/freetype && \
  ln -s /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h && \

wget -c --no-check-certificate http://downloads.sourceforge.net/project/re2c/0.16/re2c-0.16.tar.gz && \
    tar xzf re2c-0.16.tar.gz && \
    cd re2c-0.16 && \
    ./configure && make && make install && \
    cd ..  && \
    wget -c --no-check-certificate http://ftp.gnu.org/gnu/autoconf/autoconf-2.13.tar.gz && \
    tar xzf autoconf-2.13.tar.gz && \
    cd autoconf-2.13 && \
    ./configure --prefix=/usr/local/autoconf-2.13 && \
    make && make install && \
     export PHP_AUTOCONF=/usr/local/autoconf-2.13/bin/autoconf && \
    export PHP_AUTOHEADER=/usr/local/autoconf-2.13/bin/autoheader && \
    wget -c --no-check-certificate http://www.php.net/distributions/php-$PHP_VERSION.tar.gz && \
    tar xzf php-$PHP_VERSION.tar.gz && \
    cd php-$PHP_VERSION && \
     ./buildconf --force && \
    ./configure --prefix=$PHP_INSTALL_DIR --with-config-file-path=$PHP_INSTALL_DIR/etc \
        --with-config-file-scan-dir=$PHP_INSTALL_DIR/etc/php.d \
        --with-fpm-user=$RUN_USER --with-fpm-group=$RUN_USER --enable-fpm --enable-opcache \
        --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
        --with-iconv-dir=/usr/local --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib \
        --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-exif \
        --enable-sysvsem --enable-inline-optimization --with-curl=/usr/local --enable-mbregex \
        --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl \
        --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-ftp --enable-intl --with-xsl \
        --with-gettext --enable-zip --enable-soap --disable-debug && \
    sed -i '/^BUILD_/ s/\$(CC)/\$(CXX)/g' Makefile && \
    make ZEND_EXTRA_LIBS='-liconv' &&  \
    make install && \
    mkdir -p $PHP_INSTALL_DIR/etc/php.d && \
    /bin/cp php.ini-production $PHP_INSTALL_DIR/etc/php.ini && \
    /bin/cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && \
    chmod +x /etc/init.d/php-fpm && \
    rm -rf /tmp/*

# cd php-memcached
RUN wget -c --no-check-certificate https://launchpad.net/libmemcached/1.0/$LIBMEMCACHED_VERSION/+download/libmemcached-$LIBMEMCACHED_VERSION.tar.gz && \
    tar xzf libmemcached-$LIBMEMCACHED_VERSION.tar.gz && \
    cd libmemcached-$LIBMEMCACHED_VERSION && \
    sed -i "s@lthread -pthread -pthreads@lthread -lpthread -pthreads@" ./configure && \
    ./configure && \
    make && make install && \
    cd .. && \
    wget -c --no-check-certificate http://mirrors.linuxeye.com/oneinstack/src/memcached-2.2.0.tgz && \
    tar xzf memcached-2.2.0.tgz && \
    cd memcached-2.2.0 && \
    $PHP_INSTALL_DIR/bin/phpize && \
    ./configure --with-php-config=$PHP_INSTALL_DIR/bin/php-config && \
    make && make install && \
    echo "extension=memcached.so" >> $PHP_INSTALL_DIR/etc/php.d/ext-memcached.ini && \
    echo "memcached.use_sasl=1" >> $PHP_INSTALL_DIR/etc/php.d/ext-memcached.ini && \
    rm -rf /tmp/*

ADD ${REMOTE_PATH}/ext-opcache.ini $PHP_INSTALL_DIR/etc/php.d/ext-opcache.ini

# run install script
ADD ${REMOTE_PATH}/install.sh /tmp/install.sh
RUN chmod 777 install.sh && \
    bash install.sh && \
    rm -rf /tmp/*

# install composer  for php5.3
RUN curl -sS https://getcomposer.org/installer | $PHP_INSTALL_DIR/bin/php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod a+x /usr/local/bin/composer

# Set the entrypoint script.
ADD $REMOTE_PATH/entrypoint.sh /entrypoint.sh
RUN chmod 777 /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]


# Define the default command.
CMD ["bash"]