FROM tekintian/docker-base:latest
LABEL maintainer="TekinTian tekintian@gmail.com"

# This OS comes with a new autoconf tool version, which is
# not compatible with PHP, that’s why for successful compilation
# you need to temporarily install autoconf2.13 package.
WORKDIR /tmp/

ADD src/ /tmp

# Download PHP 5.2.16, Suhosin patch, PHP-FPM patch
#  - http://museum.php.net/php5/php-5.2.16.tar.bz2
#  - http://download.suhosin.org/suhosin-patch-5.2.16-0.9.7.patch.gz
#  - http://php-fpm.org/downloads/php-5.2.16-fpm-0.5.14.diff.gz


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
            zlib1g-dev \
            default-libmysqlclient-dev \
            patch \
            --no-install-recommends ' \
    && apt-get update && apt-get install -y ${buildDeps} && rm -rf /var/lib/apt/lists/*  && \
    cd /tmp/  && \
   # wget -c --no-check-certificate http://ftp.gnu.org/gnu/autoconf/autoconf-2.13.tar.gz && \
    tar xzf autoconf-2.13.tar.gz && \
    cd autoconf-2.13 && \
    ./configure --prefix=/usr/local/autoconf-2.13 && \
    make && make install && \
    export PHP_AUTOCONF=/usr/local/autoconf-2.13/bin/autoconf && \
    export PHP_AUTOHEADER=/usr/local/autoconf-2.13/bin/autoheader  && \
    cd /tmp/  && \
    tar xzf sqlite-autoconf-3230000.tar.gz && \
    cd sqlite-autoconf-3230000 && \
    ./configure --prefix=/usr/local/sqlite && \
    make && make install && \
#zlib-1.2.11
    cd /tmp/  && \
    tar xzf zlib-1.2.11.tar.gz && \
    cd zlib-1.2.11 && \
     ./configure --prefix=/usr/local/zlib && \
   # cmake . -DCMAKE_INSTALL_PREFIX=/usr/lib/libpng163 && \
    make && make install && \
    cd /tmp/  && \
    #libpng
    cd /tmp/  && \
    tar xzf libpng-master.tar.gz && \
    cd libpng-master && \
     ./configure --prefix=/usr/local/libpng && \
   # cmake . -DCMAKE_INSTALL_PREFIX=/usr/lib/libpng163 && \
    make && make install && \
    ln -s /usr/local/libpng/lib/libpng.* /usr/lib/ && \
    cd /tmp/  && \
    tar zxf php-5.2.17.tar.gz  && \
    cd /tmp/php-5.2.17 && \
# Apply patches
    patch -p1 -i ../php-5.2.17-fpm-0.5.14.diff && \
    patch -p1 -i ../suhosin-patch-5.2.17-0.9.7.patch  && \
# Configure
 ./buildconf --force  && \
./configure \
    --enable-fastcgi \
    --enable-fpm \
    --enable-mbstring \
    --enable-sockets \
    --enable-pdo=shared \
    --with-config-file-path=/etc/php \
    --with-curl \
    --with-fpm-conf=/etc/php/php-fpm.conf \
    --with-fpm-log=/var/log/php/php_errors.log \
    --with-fpm-pid=/var/run/php/php-fpm.pid \
    --with-gd \
    --with-gettext \
    --with-libdir \
    --with-libdir=lib \
    --with-mcrypt \
    --with-mhash \
    --with-mysql=/tmp/libmysql \
    --with-mysqli=/tmp/libmysql \
    --with-pdo-mysql=/tmp/libmysql \
    --with-openssl \
    --with-pcre-regex \
    --with-zlib=/usr/local/zlib  \
    --with-sqlite=/usr/local/sqlite \
    --with-pdo-sqlite=/usr/local/sqlite && \
    # Install
    make && make install && \
    # Clean up
     cd /tmp/ && rm -rf /tmp/* && \
    #remove build deps
    apt-get purge -y --auto-remove ${buildDeps}

# Get out of /tmp
WORKDIR /

# Add entrypoint
COPY init.sh /init
ENTRYPOINT ["/init"]

# Run php-fpm
EXPOSE 9000
CMD ["php-cgi", "--fpm"]