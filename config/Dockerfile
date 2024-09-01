# PHPとDebianベースのDockerイメージ
FROM php:8.3-fpm-bookworm

# OSパッケージインストール
RUN apt update && apt upgrade -y
RUN apt install -y \
    nginx \
    supervisor \
    bash \
    vim \
    git \
    zip \
    unzip \
    libcurl4-openssl-dev \
    libzip-dev \
    libxml2-dev \
    libonig-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    redis-server

# PHPの拡張機能をインストール
RUN docker-php-ext-install mysqli pdo pdo_mysql curl zip xml mbstring intl gd bcmath soap opcache pcntl exif fileinfo

# OSパッケージクリーン
RUN apt clean
RUN rm -rf /var/lib/apt/lists/*

# PHP設定
COPY php.ini /usr/local/etc/php

# Nginx設定
COPY nginx.conf /etc/nginx/sites-available/default

# Supervisor設定
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Bashにエイリアスを追加
RUN echo 'alias ll="ls -la"' >> ~/.bashrc

# Vim調整
RUN echo ':set encoding=utf-8' >> ~/.vimrc
RUN echo 'syntax on' >> ~/.vimrc

# NVMからNode.jsインストール
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash && \
    bash -c "source ~/.bashrc && nvm install --lts"

# Composerインストール
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

WORKDIR /var/www/html

# Supervisorで各プロセス起動
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]