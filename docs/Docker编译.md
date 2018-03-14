#Build

***
##基础包

	cd /Users/Github/docker-base && docker build -t tekintian/docker-base .

##php

	cd /Users/Github/php/php-5.6 && docker build -t tekintian/php:5.6.34 .

	cd /Users/Github/php/php-7.0 && docker build -t tekintian/php:7.0.28 .

	cd /Users/Github/php/php-7.1 && docker build -t tekintian/php:7.1.15 .

	cd /Users/Github/php/php-7.2 && docker build -t tekintian/php:7.2.3 .


## tengine

cd /Users/Github/tengine-php/php-7.2-waf && docker build -t tekintian/tengine-php:7.2.3-waf .
cd /Users/Github/tengine-php/php-7.2 && docker build -t tekintian/tengine-php:7.2.3 .

cd /Users/Github/tengine-php/php-7.1 && docker build -t tekintian/tengine-php:7.1.15 .
cd /Users/Github/tengine-php/php-7.1-waf && docker build -t tekintian/tengine-php:7.1.15-waf .

cd /Users/Github/tengine-php/php-7.0 && docker build -t tekintian/tengine-php:7.0.28 .
cd /Users/Github/tengine-php/php-7.0-waf && docker build -t tekintian/tengine-php:7.0.28-waf .

cd /Users/Github/tengine-php/php-5.6 && docker build -t tekintian/tengine-php:5.6.34 .

docker push tekintian/tengine-php:7.0.28-waf



docker tag tekintian/tengine-php:7.2.3-waf tekintian/tengine-php:latest-waf

docker tag tekintian/php:7.0.28 tekintian/php:7.0
docker tag tekintian/php:7.2.3 tekintian/php:latest


docker push tekintian/php:latest
docker push tekintian/tengine-php:latest-waf



docker inspect –format=’{{.Id}} {{.Parent}}’ $(docker images –filter since=4aed682152b1 -q)

***
#Run：

docker run -it -d --name base -v /Users/Github/diy:/diy tekintian/docker-base
docker exec -it base bash

docker run -it -d --name php70 -v /Users/Github/diy:/diy tekintian/php:7.0.28
docker exec -it php70 bash

docker run -it -d --name php72 -v /Users/Github/diy:/diy tekintian/php:7.2.3
docker exec -it php72 bash



