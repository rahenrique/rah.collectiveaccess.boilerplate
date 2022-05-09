FROM webdevops/php-nginx:7.4-alpine

RUN apt-get update && apt-get install -y \
    default-mysql-client

WORKDIR /var/www/html/pawtucket

RUN git clone --depth 1 --branch 1.7.14 https://github.com/collectiveaccess/pawtucket2.git .

# RUN mkdir /var/www/html/pawtucket/app/tmp/collectiveaccessCache && \
#     mkdir /var/www/html/pawtucket/media/collectiveaccess

# RUN chmod -R 777 /var/www/html/pawtucket/app/tmp && \
#     chmod -R 777 /var/www/html/pawtucket/app/log && \
#     chmod -R 777 /var/www/html/pawtucket/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache && \
#     chmod -R 777 /var/www/html/pawtucket/media

ENV WEB_DOCUMENT_ROOT=/var/www/html/pawtucket
ENV php.memory_limit=2048M
ENV PHP_MEMORY_LIMIT=2048M
ENV PHP_MAX_EXECUTION_TIME=1200
ENV PHP_POST_MAX_SIZE=50M
ENV PHP_UPLOAD_MAX_FILESIZE=50M
ENV PHP_DISPLAY_ERRORS=1