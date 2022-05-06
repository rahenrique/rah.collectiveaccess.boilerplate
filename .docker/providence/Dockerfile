FROM webdevops/php-nginx-dev:7.4

RUN apt-get update && apt-get install -y \
    default-mysql-client

WORKDIR /var/www/html/providence

RUN git clone --depth 1 --branch 1.7.14 https://github.com/collectiveaccess/providence.git .

RUN mkdir /var/www/html/providence/app/tmp/collectiveaccessCache && \
    mkdir /var/www/html/providence/media/collectiveaccess

RUN chmod -R 777 /var/www/html/providence/app/tmp && \
    chmod -R 777 /var/www/html/providence/app/log && \
    chmod -R 777 /var/www/html/providence/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache && \
    chmod -R 777 /var/www/html/providence/media

# This is the default entrypoint location for the webdevops images
# https://dockerfile.readthedocs.io/en/latest/content/Customization/provisioning.html
# COPY ./.docker/providence/entrypoint.sh /opt/docker/provision/entrypoint.d/99-userscript.sh

ENV WEB_DOCUMENT_ROOT=/var/www/html/providence
ENV php.memory_limit=2048M
ENV PHP_MEMORY_LIMIT=2048M
ENV PHP_MAX_EXECUTION_TIME=1200
ENV PHP_POST_MAX_SIZE=50M
ENV PHP_UPLOAD_MAX_FILESIZE=50M
ENV PHP_DISPLAY_ERRORS=1