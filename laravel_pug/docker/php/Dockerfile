FROM php:7-fpm

RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql

RUN    curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# 必要なツール郡をインストール
RUN apt-get update \
    && apt-get upgrade -y sendmail \
    && rm -rf /var/lib/apt/lists/*

    #user追加
RUN  useradd -m -s /bin/bash -u 1000 styleb

    #www.conf修正
RUN    sed -i 's/user\ \=\ www-data/user\ \=\ styleb/g' /usr/local/etc/php-fpm.d/www.conf && \
    sed -i 's/group\ \=\ www-data/group\ \=\ styleb/g' /usr/local/etc/php-fpm.d/www.conf
WORKDIR /var/www
