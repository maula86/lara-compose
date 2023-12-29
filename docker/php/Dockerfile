FROM php:8.2-fpm

ARG user
ARG uid

ENV TZ=Asia/Jakarta
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update
RUN apt-get install -y openssl zip unzip git curl 
# supervisor
RUN apt-get install -y libzip-dev libonig-dev libicu-dev
RUN apt-get install -y autoconf pkg-config libssl-dev

RUN docker-php-ext-install bcmath mbstring intl opcache pcntl

RUN docker-php-ext-install pdo pdo_mysql mysqli

# Install Node NPM
# RUN apt-get install -yq nodejs build-essential
# RUN apt-get install -y nodejs
# RUN npm install -g npm

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

USER $user

# RUN npm install -g pm2

# COPY start-container /usr/local/bin/start-container
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY php.ini /etc/php/8.2/cli/conf.d/mscustomphp.ini
# RUN chmod +x /usr/local/bin/start-container