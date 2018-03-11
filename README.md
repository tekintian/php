php for docker

    PHP + ImageMagick + ZendOpcache
    Memcache + Memcached + Redis + Swoole

    ZendGuardLoader + ionCube for php5.6

## usage

### php control
start|stop|restart|reload|status
```
docker exec -d php service php-fpm restart
```

PHP开发交流
http://github.com/ynphp


Tekin

http://dev.yunnan.ws



PHP_VERSION=7.2.3 \
    IMAGICK_PECL_VERSION=3.4.3 \
    LIBMEMCACHED_VERSION=1.0.18 \
    MEMCACHE_PECL_VERSION=3.0.8 \
    REDIS_PECL_VERSION=3.1.6 \
    SWOOLE_VERSION=2.1.1 \
    OAUTH_PECL_VERSION=2.0.2 \
    SPHINX_PECL_VERSION=1.3.3 \
    GNUPG_VERSION=1.4.0 \


http://cn2.php.net/distributions/php-7.2.3.tar.gz
http://www.imagemagick.org/download/ImageMagick.tar.gz
http://pecl.php.net/get/imagick-3.4.3.tgz
https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz
https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz -O php-memcached-php7.tar.gz
http://pecl.php.net/get/redis-3.1.6.tgz
http://pecl.php.net/get/oauth-2.0.2.tgz 
https://nchc.dl.sourceforge.net/project/re2c/1.0.1/re2c-1.0.1.tar.gz
http://pecl.php.net/get/swoole-2.1.1.tgz
http://pecl.php.net/get/gnupg-1.4.0.tgz
https://nchc.dl.sourceforge.net/project/re2c/1.0.1/re2c-1.0.1.tar.gz
http://sphinxsearch.com/files/sphinx-2.2.11-release.tar.gz

https://github.com/tekintian/php/raw/master/src/sphinx-php7-339e123.tar.gz


RUN set -x \
		#install build deps
		&& apt-get update && apt-get install -y gcc g++ make cmake autoconf libssl-dev  libcurl4-openssl-dev libxslt-dev libicu-dev  libxml2-dev libjpeg-dev libpng-dev libfreetype6-dev libsasl2-dev libevent-dev libpcre3-dev libgpgme11-dev pkg-config patch && rm -rf /var/lib/apt/lists/* \

		......


		#remove build deps
		&& apt-get purge -y --auto-remove gcc g++ make cmake autoconf libssl-dev  libcurl4-openssl-dev libxslt-dev libicu-dev  libxml2-dev libjpeg-dev libpng-dev libfreetype6-dev libsasl2-dev libevent-dev  libpcre3-dev libgpgme11-dev pkg-config patch libgpgme11-dev \
		&& apt-get clean && apt-get autoclean  \
		&& cd /tmp/  &&  rm -rf /tmp/* 


# 查找删除无用的文件
  # && rm -rf /usr/local/mysql/bin/*-debug /usr/local/mysql/bin/*_embedded \
        # && find /usr/local/mysql -type f -name "*.a" -delete \
        # && apt-get update && apt-get install -y binutils && rm -rf /var/lib/apt/lists/* \
        # && { find /usr/local/mysql -type f -executable -exec strip --strip-all '{}' + || true; } \
        # && apt-get purge -y --auto-remove binutils \
        # && sed -i 's/mirrors.aliyun.com/deb.debian.org/g' /etc/apt/sources.list

