FROM qxdock/php-fpm:8.0

RUN curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh; \
    bash nodesource_setup.sh; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
    nodejs \
    nginx; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*; \
    npm i -g npm yarn; \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"; \
    php composer-setup.php; \
    php -r "unlink('composer-setup.php');"; \
    mv composer.phar /usr/local/bin/composer;

COPY nginx.conf /etc/nginx/nginx.conf
COPY php-fpm/www.conf /usr/local/etc/php-fpm.d/www.conf
COPY php-fpm/zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf
COPY run.sh /opt/run.sh

RUN chmod +x /opt/run.sh; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*;

ENTRYPOINT ["/opt/run.sh"]
