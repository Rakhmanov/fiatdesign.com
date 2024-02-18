+++
title = "Installing PHP7 pecl extensions on Alpine 3.4 Docker Images"
date = "2018-02-15"
author = "Principal"
description = ""
tags = ["Docker"]
categories = ["DevOps"]
+++


Problem:
Cant install pecl&nbsp;packages on Alpine PHP7 Docker Image.

Since PHP Container is based on Alpine 3.4 which in turn does not have library memcached available.

It seems Alpine 3.4 is meant to work with PHP5.
Thus creating a gap with the availability of packages for PHP7 and up. Luckily newer versions are switching to the alpine 3.7 where all the packages already available from the distro's package source.

However this step in your Dockerfile will allow you to install anything you like with pecl on Alpine.
<pre>RUN apk update\
  &amp;&amp; apk upgrade \
  &amp;&amp; apk add libmemcached \
    libmemcached-libs \
    libmemcached-dev \
    build-base \
    zlib-dev \
    php5-dev \
    git \
    autoconf \
    cyrus-sasl-dev \
  &amp;&amp; pecl config-set php_ini  /usr/local/etc/php/php.ini \
  &amp;&amp; pecl install -f memcached \ #Or any Additional packages
  &amp;&amp; echo extension=memcached.so &gt;&gt; /usr/local/etc/php/conf.d/docker-php-ext-memcached.ini \
  &amp;&amp; rm -rf /tmp/pear \
  &amp;&amp; apk del php5-dev \
     build-base \
     zlib-dev \
     php5-dev \
     git \
     autoconf \
     cyrus-sasl-dev
</pre>
Where command <code>pecl install -f</code> runs you can install extensions necessary such as memcached, xdebug or anything you like from&nbsp;http://pecl.php.net/ .

And don't forget to include your package, by creating file inside of the new conf.d directory.
So to resolve the deficiency I have been using a work around where I install pecl with PHP5 and make all necessary extension installations for the PHP7, and cleanup afterward.

Images affected:
<pre> 7.1.14-cli-alpine3.4, 7.1-cli-alpine3.4, 7.1.14-alpine3.4, 7.1-alpine3.4, 7.1.14-cli-alpine, 7.1-cli-alpine, 7.1.14-alpine, 7.1-alpine (7.1/alpine3.4/cli/Dockerfile)
 7.1.14-fpm-alpine3.4, 7.1-fpm-alpine3.4, 7.1.14-fpm-alpine, 7.1-fpm-alpine (7.1/alpine3.4/fpm/Dockerfile)
 7.1.14-zts-alpine3.4, 7.1-zts-alpine3.4, 7.1.14-zts-alpine, 7.1-zts-alpine (7.1/alpine3.4/zts/Dockerfile)
</pre>
<pre>7.0.27-cli-alpine3.4, 7.0-cli-alpine3.4, 7.0.27-alpine3.4, 7.0-alpine3.4, 7.0.27-cli-alpine, 7.0-cli-alpine, 7.0.27-alpine, 7.0-alpine (7.0/alpine3.4/cli/Dockerfile)
7.0.27-fpm-alpine3.4, 7.0-fpm-alpine3.4, 7.0.27-fpm-alpine, 7.0-fpm-alpine (7.0/alpine3.4/fpm/Dockerfile)
7.0.27-zts-alpine3.4, 7.0-zts-alpine3.4, 7.0.27-zts-alpine, 7.0-zts-alpine (7.0/alpine3.4/zts/Dockerfile)
</pre>
And any derivatives. Such as:
<pre>wordpress:php7.1-fpm-alpine
wordpress:php7.0-fpm-alpine
wordpress:php7.1-cli-alpine
wordpress:php7.0-cli-alpine
</pre>