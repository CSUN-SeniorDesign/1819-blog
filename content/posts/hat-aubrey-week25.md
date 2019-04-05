---
title: "Hat Aubrey Week25"
date: 2019-04-04T20:29:50-07:00
draft: false
---

# Installing default and custom memcached client #

The official php image comes with many php extensions compressed to save space. An admin may install, enable and configure them using the   

	docker-php-ext-install
	docker-php-ext-configure
	docker-php-ext-enable

Initially, I installed the default memcached provided in the image by:

	apt-get install libmemcached-dev \
    zlib1g-dev;
	pecl install memcached-3.1.3;
	docker-php-ext-enable memcached

This allowed the w3tc plugin to use memcache for database cache. However, since we are using the AWS elasticcache, we needed to use the amazon memcached client. 

The caveat is that the highest version of php it supports is 7.2.16. It wasn't a big deal as I just changed the version in the dockerfile.

The instruction on how to install it is generic enough that I needed to gather some information first.

To do so:
1. I started the container
2. I connected to the container using docker exec -it containername bash
3. php -i | grep extension_dir
4. php -i | grep php.ini

Number 3, gave me the directory of the php extensions. The php 7.2 and 7.3 images have slightly different directory of extension.

Number 4 gave me the location of where I can edit the php configuration files

With this information, I proceeded to modify the docker file to install the aws memcached

	apt-get install wget \
    gcc \
    g++ \
	; \
	wget https://elasticache-downloads.s3.amazonaws.com/ClusterClient/PHP-7.2/latest-64bit; \
	tar -zxvf latest-64bit; \
    mv amazon-elasticache-cluster-client.so /usr/local/lib/php/extensions/no-debug-non-zts-20170718/; \
	echo "extension=amazon-elasticache-cluster-client.so" | tee --append /usr/local/etc/php/php.ini; \ 


