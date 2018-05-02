#Docker Build命令

***
##基础包

	cd /Users/Github/docker-base && docker build -t tekintian/docker-base .

##php

	cd /Users/Github/php/php-5.6 && docker build -t tekintian/php:5.6.36 .

	cd /Users/Github/php/php-7.0 && docker build -t tekintian/php:7.0.30 .

	cd /Users/Github/php/php-7.1 && docker build -t tekintian/php:7.1.17 .

	cd /Users/Github/php/php-7.2 && docker build -t tekintian/php:7.2.5 .

	cd /Users/Github/php/php-7.3-dev && docker build -t tekintian/php:7.3-build20180428 .


## tengine

cd /Users/Github/tengine-php/php-7.2-waf && docker build -t tekintian/tengine-php:7.2.5-waf .
cd /Users/Github/tengine-php/php-7.2 && docker build -t tekintian/tengine-php:7.2.5 .

cd /Users/Github/tengine-php/php-7.1 && docker build -t tekintian/tengine-php:7.1.17 .
cd /Users/Github/tengine-php/php-7.1-waf && docker build -t tekintian/tengine-php:7.1.17-waf .

cd /Users/Github/tengine-php/php-7.0 && docker build -t tekintian/tengine-php:7.0.30 .
cd /Users/Github/tengine-php/php-7.0-waf && docker build -t tekintian/tengine-php:7.0.30-waf .

cd /Users/Github/tengine-php/php-5.6 && docker build -t tekintian/tengine-php:5.6.36 .


#nginx-php

cd /Users/Github/nginx-php/7.3-waf && docker build -t tekintian/nginx-php:7.3-build20180428 .

cd /Users/Github/nginx-php/7.2-waf && docker build -t tekintian/nginx-php:7.2.5-waf .
cd /Users/Github/nginx-php/7.2 && docker build -t tekintian/nginx-php:7.2.5 .

cd /Users/Github/nginx-php/7.1 && docker build -t tekintian/nginx-php:7.1.17 .
cd /Users/Github/nginx-php/7.1-waf && docker build -t tekintian/nginx-php:7.1.17-waf .

cd /Users/Github/nginx-php/7.0 && docker build -t tekintian/nginx-php:7.0.30 .
cd /Users/Github/nginx-php/7.0-waf && docker build -t tekintian/nginx-php:7.0.30-waf .

cd /Users/Github/nginx-php/5.6 && docker build -t tekintian/nginx-php:5.6.36 .

## 推送
docker push tekintian/nginx-php:7.3-build20180428
docker push tekintian/nginx-php:7.2.5-waf
docker push tekintian/nginx-php:7.2.5
docker push tekintian/nginx-php:7.1.17-waf
docker push tekintian/nginx-php:7.1.17
docker push tekintian/nginx-php:7.0.30-waf
docker push tekintian/nginx-php:7.0.30
docker push tekintian/nginx-php:5.6.36


***
#Run：

docker run -it -d --name base -v /Users/Github/diy:/diy tekintian/docker-base
docker exec -it base bash

docker run -it -d --name php70 -v /Users/Github/diy:/diy tekintian/php:7.0.28
docker exec -it php70 bash

docker run -it -d --name php72 -v /Users/Github/diy:/diy tekintian/php:7.2.3
docker exec -it php72 bash



