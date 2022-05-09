FROM ubuntu:20.04

ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_LOG_DIR=/var/log/apache2
ENV APACHE_PID_FILE=/var/run/apache2.pid
ENV APACHE_RUN_DIR=/var/run/apache2
ENV APACHE_LOCK_DIR=/var/lock/apache2
ENV APACHE_LOG_DIR=/var/log/apache2

ENV WEB_DOCUMENT_ROOT=/var/www
ENV CA_PROVIDENCE_VERSION=1.7.14
ENV CA_PROVIDENCE_DIR=${WEB_DOCUMENT_ROOT}/providence
ENV CA_PAWTUCKET_VERSION=1.7.14
ENV CA_PAWTUCKET_TEMP_DIR=${WEB_DOCUMENT_ROOT}/pawtucket
ENV CA_PAWTUCKET_DIR=${WEB_DOCUMENT_ROOT}
ENV TZ=America/Sao_Paulo

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

RUN apt-get update && \
    apt-get install -y apache2 \
    php7.4 \
    libapache2-mod-php7.4 \
    curl \
    php-mysql \
    mysql-client \
    curl \
    php7.4-curl \
    php7.4-xml \
    zip \
    wget \
    ffmpeg \
    ghostscript \
    imagemagick \
    php7.4-gd \
    libreoffice \
    php7.4-zip \
    git


# GMAGICK
RUN apt-get install -y php-pear \
    php7.4-dev \
    graphicsmagick \
    libgraphicsmagick1-dev \
	&& pecl install gmagick-2.0.4RC1

WORKDIR ${WEB_DOCUMENT_ROOT}

# PROVIDENCE
RUN git clone --depth 1 --branch ${CA_PROVIDENCE_VERSION} https://github.com/collectiveaccess/providence.git ${CA_PROVIDENCE_DIR}

RUN mkdir ${CA_PROVIDENCE_DIR}/app/tmp/collectiveaccessCache && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess

RUN chmod -R 777 ${CA_PROVIDENCE_DIR}/app/tmp && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/app/log && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/media


# PAWTUCKET
RUN git clone --depth 1 --branch ${CA_PAWTUCKET_VERSION} https://github.com/collectiveaccess/pawtucket2.git ${CA_PAWTUCKET_TEMP_DIR}
RUN mv ${CA_PAWTUCKET_TEMP_DIR}/* ${CA_PAWTUCKET_DIR}
RUN rm -rf ${CA_PAWTUCKET_TEMP_DIR}
RUN ln -s ${CA_PROVIDENCE_DIR}/media ${CA_PAWTUCKET_DIR}/media

RUN chmod -R 777 ${CA_PAWTUCKET_DIR}/app/tmp && \
    chmod -R 777 ${CA_PAWTUCKET_DIR}/app/log && \
    chmod -R 777 ${CA_PAWTUCKET_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache


# APACHE WEB SERVER
COPY ./.docker/providence/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf
# RUN sed -i "s@DocumentRoot \/var\/www\/html@DocumentRoot \/var\/www@g" /etc/apache2/sites-available/000-default.conf
RUN rm -rf /var/www/html
RUN chown -R www-data:www-data ${WEB_DOCUMENT_ROOT}

# COPY php.ini /etc/php/7.4/apache2/php.ini
# COPY entrypoint.sh /entrypoint.sh
# RUN chmod 777 /entrypoint.sh
# ENTRYPOINT ["/entrypoint.sh"]

CMD [ "/usr/sbin/apache2", "-DFOREGROUND" ]






# ENV WEB_DOCUMENT_ROOT=/var/www/html/providence
# ENV PHP_MEMORY_LIMIT=2048M
# ENV PHP_MAX_EXECUTION_TIME=1200
# ENV PHP_POST_MAX_SIZE=50M
# ENV PHP_UPLOAD_MAX_FILESIZE=50M
# ENV PHP_DISPLAY_ERRORS=1

# ENV CA_PROVIDENCE_VERSION=1.7.14
# ENV CA_PROVIDENCE_DIR=${WEB_DOCUMENT_ROOT}
# ENV CA_PAWTUCKET_VERSION=1.7.14
# ENV CA_PAWTUCKET_DIR=${WEB_DOCUMENT_ROOT}/pawtucket

# RUN apt-get update && apt-get install -y \
#     # default-mysql-client \
#     ffmpeg \
#     ghostscript \
#     imagemagick \
#     libreoffice

# WORKDIR ${CA_PROVIDENCE_DIR}


# # PROVIDENCE
# RUN git clone --depth 1 --branch ${CA_PROVIDENCE_VERSION} https://github.com/collectiveaccess/providence.git ${CA_PROVIDENCE_DIR}

# RUN mkdir ${CA_PROVIDENCE_DIR}/app/tmp/collectiveaccessCache && \
#     mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess

# RUN chmod -R 777 ${CA_PROVIDENCE_DIR}/app/tmp && \
#     chmod -R 777 ${CA_PROVIDENCE_DIR}/app/log && \
#     chmod -R 777 ${CA_PROVIDENCE_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache && \
#     chmod -R 777 ${CA_PROVIDENCE_DIR}/media


# # PAWTUCKET
# RUN git clone --depth 1 --branch ${CA_PAWTUCKET_VERSION} https://github.com/collectiveaccess/pawtucket2.git ${CA_PAWTUCKET_DIR}
# # # RUN mv ${CA_PAWTUCKET_TEMP_DIR}/* ${CA_PAWTUCKET_DIR}
# RUN ln -s ${CA_PROVIDENCE_DIR}/media ${CA_PAWTUCKET_DIR}/media

# RUN chmod -R 777 ${CA_PAWTUCKET_DIR}/app/tmp && \
#     chmod -R 777 ${CA_PAWTUCKET_DIR}/app/log && \
#     chmod -R 777 ${CA_PAWTUCKET_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache


# PERMISSIONS FOR WEB USER
# RUN chown -R www-data:www-data ${WEB_DOCUMENT_ROOT}

# This is the default entrypoint location for the webdevops images
# https://dockerfile.readthedocs.io/en/latest/content/Customization/provisioning.html
# COPY ./.docker/providence/entrypoint.sh /opt/docker/provision/entrypoint.d/99-userscript.sh
