FROM tekintian/docker-base:latest
MAINTAINER Tekin <tekintian@gmail.com>

ADD tmp/ /tmp

# This OS comes with a new autoconf tool version, which is
# not compatible with PHP, that’s why for successful compilation
# you need to temporarily install autoconf2.13 package.autoconf2.13 \
RUN rm -rf /etc/apt && mv /tmp/apt /etc/  \
    &&  apt-get update \
    && apt-get install -y \
        libbz2-dev \
        autoconf2.13 \
        libcurl3-openssl-dev \
        libltdl-dev \
        libmcrypt-dev \
        libevent-dev \
        libmhash-dev \
        libmysqlclient-dev \
        libpcre3-dev \
        libpng-dev \
        libxml2-dev \
        libcurl3 \
        make \
        patch \
        xmlstarlet && \
     apt-get clean && \
    cd /tmp/  && rm -rf /var/lib/apt/lists/*
   # wget -c --no-check-certificate http://ftp.gnu.org/gnu/autoconf/autoconf-2.13.tar.gz && \
    # tar xzf autoconf-2.13.tar.gz && \
    # cd autoconf-2.13 && \
    # ./configure --prefix=/usr/local/autoconf-2.13 && \
    # make && make install && \
    # export PHP_AUTOCONF=/usr/local/autoconf-2.13/bin/autoconf && \
    # export PHP_AUTOHEADER=/usr/local/autoconf-2.13/bin/autoheader  && \
    # cd /tmp/  && \
    #  rm -rf /var/lib/apt/lists/*

# Download PHP 5.2.16, Suhosin patch, PHP-FPM patch
#  - http://museum.php.net/php5/php-5.2.16.tar.bz2
#  - http://download.suhosin.org/suhosin-patch-5.2.16-0.9.7.patch.gz
#  - http://php-fpm.org/downloads/php-5.2.16-fpm-0.5.14.diff.gz

RUN tar zxf /tmp/php-5.2.17.tar.gz -C /tmp
WORKDIR /tmp/php-5.2.17

# Apply patches
RUN patch -p1 -i ../php-5.2.17-fpm-0.5.14.diff && \
    patch -p1 -i ../suhosin-patch-5.2.17-0.9.7.patch 


# Configure
RUN ./buildconf --force
RUN ./configure \
    --enable-fastcgi \
    --enable-fpm \
    --enable-mbstring \
    --enable-sockets \
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
    --with-mysql \
    --with-mysql-sock \
    --with-mysqli \
    --with-openssl \
    --with-pcre-regex \
    --with-png-dir \
    --with-zlib \
    --without-sqlite

# Install
RUN make && make install

# Uninstall autoconf2.13 after compilation.
RUN apt-get remove -y autoconf2.13

# Clean up
RUN rm -rf /tmp/* /var/tmp/*

# Get out of /tmp
WORKDIR /

# Add entrypoint
COPY init.sh /init
ENTRYPOINT ["/init"]

# Run php-fpm
EXPOSE 9000
CMD ["php-cgi", "--fpm"]
