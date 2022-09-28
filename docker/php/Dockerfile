FROM davidschneiderinfo/php:latest

ADD ./docker/php/laravel.ini /usr/local/etc/php/conf.d

#####################################
# Aliases:
#####################################

# docker-compose exec php artisan --> php artisan
RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan "$@"' > /usr/bin/artisan
RUN chmod +x /usr/bin/artisan

# docker-compose exec test --> run the tests for the project and generate testdox
RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan config:clear\n/var/www/vendor/bin/phpunit -d memory_limit=2G --stop-on-error --stop-on-failure --testdox-text=tests/report.txt "$@"' > /usr/bin/t
RUN chmod +x /usr/bin/test

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

ADD ./docker/php/php-fpm.conf /usr/local/etc/php-fpm.conf

WORKDIR /var/www

COPY ./docker/php/docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
RUN ln -s /usr/local/bin/docker-entrypoint.sh /
ENTRYPOINT ["docker-entrypoint.sh"]

COPY . /var/www
RUN chown -R www-data /var/www/storage
RUN chmod 755 -R /var/www/storage

EXPOSE 9000
CMD ["php-fpm"]
