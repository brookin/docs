LAMP 环境编译

- date:2015/8/19 12:50:53
- date:2017/6/29 支持PHP7.1.6

## apache

## mysql

	ln -s /usr/local/services/mysql-5.6.24 /usr/local/services/mysql

## php

编译选项

	./configure \
	--prefix=/usr/local/services/php-7.1.6 \
	--with-mysqli=/usr/local/services/mysql/bin/mysql_config \
	--with-pdo-mysql=/usr/local/services/mysql \
	--with-apxs2=/usr/local/services/httpd/bin/apxs \
	--with-libxml-dir=/usr/local/services/libxml2 \
	--with-gd=/usr/local/services/libgd \
	--with-freetype-dir=/usr/local/services/freetype \
	--with-png-dir=/usr/local/services/libpng \
	--with-jpeg-dir=/usr/local/services/jpeg \
	--with-zlib-dir=/usr/local/services/zlib \
	--with-curl=/usr/local/services/curl \
	--with-icu-dir=/usr/local/services/icu \
	--with-xpm-dir=/usr/local/services/libXpm \
	--enable-mbstring \
	--enable-sockets \
	--enable-pcntl \
	--enable-sysvmsg \
	--enable-sysvsem \
	--enable-sysvshm \
	--enable-soap \
	--enable-shmop \
	--enable-bcmath \
	--enable-zip \
	--enable-calendar \
	--enable-ctype \
	--enable-dom \
	--enable-filter \
	--enable-gd-native-ttf \
	--enable-hash \
	--enable-json \
	--enable-posix \
	--enable-shared \
	--enable-simplexml \
	--enable-static \
	--enable-tokenizer \
	--enable-xml \
	--enable-xmlwriter \
	--enable-pdo \
	--enable-inline-optimization \
	--enable-mysqlnd \
	--enable-exif \
	--enable-intl

## 可选项

	--enable-maintainer-zts // 线程安全
	--with-mysql=/usr/local/services/mysql 	// php 7 原生已经去除了对这个版本 mysql 驱动的支持

## 注意事项

1. php 5.5 need >= gd-2.1.0
```
./configure \
--prefix=/usr/local/services/gd-2.1.1 \
--with-freetype=/usr/local/services/freetype \
--with-png=/usr/local/services/libpng \
--with-jpeg=/usr/local/services/jpeg
```

2. icu 的支持


## 相关资源下载地址

- icu: http://icu-project.org/download/4.2.html
- gd: https://github.com/libgd/libgd/archive/gd-2.1.1.tar.gz
