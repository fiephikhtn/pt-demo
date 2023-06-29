FROM php:8.2-fpm
WORKDIR /var/www/html

RUN apt-get update && apt-get install -y && apt-get -y install sudo \
    build-essential \
    mariadb-client \
    libzip-dev \
    libonig-dev \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libmagickwand-dev \
    iputils-ping \
    && docker-php-ext-install pdo_mysql mbstring zip exif pcntl \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Imagick extension
RUN pecl install imagick \
    && docker-php-ext-enable imagick

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# install cron
RUN apt-get update && apt-get install -y cron
# Copy cron-batch file to the cron.d directory
COPY ./php/cron-batch /etc/cron.d/cron-batch
# Give execution rights on the cron job
RUN chmod 0644 /etc/cron.d/cron-batch
# Apply cron job
RUN crontab /etc/cron.d/cron-batch
# Create the log file to be able to run tail
RUN touch /var/log/cron.log
# Run the command on container startup
#CMD cron && tail -f /var/log/cron.log

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# start add 2021/04/14
# Disable password username www
RUN passwd --delete www

# Ensure sudo group users are not
# asked for a password when using
# sudo command by ammending sudoers file
RUN echo '%www ALL=(ALL) NOPASSWD:ALL' >> \
    /etc/sudoers
# end add 2021/04/14

# Copy existing application directory contents
COPY . /var/www/html

# Copy existing application directory permissions
COPY --chown=www:www . /var/www/html

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]