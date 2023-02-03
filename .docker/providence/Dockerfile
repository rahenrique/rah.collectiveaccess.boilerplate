# --------------------------------------------------------------------------------
# "ca-ubuntu-base" stage with environment variables
FROM ubuntu:20.04 as ca-ubuntu-base

ENV WEB_DOCUMENT_ROOT=/var/www/html
ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_LOG_DIR=/var/log/apache2
ENV APACHE_PID_FILE=/var/run/apache2.pid
ENV APACHE_RUN_DIR=/var/run/apache2
ENV APACHE_LOCK_DIR=/var/lock/apache2
ENV APACHE_LOG_DIR=/var/log/apache2
ENV TIME_ZONE=America/Sao_Paulo

RUN ln -snf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && echo ${TIME_ZONE} > /etc/timezone



# --------------------------------------------------------------------------------
# "ca-builder-base" stage with dependency instalation
FROM ca-ubuntu-base as ca-builder-base

RUN apt-get update && apt-get install -y apache2 \
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
    dcraw \
    ghostscript \
    imagemagick \
    php7.4-gd \
    libreoffice \
    php7.4-zip \
    php7.4-intl \
    git-all \
    php-bcmath \
    && a2enmod rewrite \
    && service apache2 restart

# GMAGICK
RUN apt-get install -y php-pear \
    php7.4-dev \
    graphicsmagick \
    libgraphicsmagick1-dev \
	&& pecl install gmagick-2.0.4RC1

# PHP Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

WORKDIR ${WEB_DOCUMENT_ROOT}



# --------------------------------------------------------------------------------
# "ca-pawtucket-base" stage installs the pawtucket application
FROM ca-builder-base as ca-pawtucket-base

ENV CA_PAWTUCKET_DIR=${WEB_DOCUMENT_ROOT}
ENV CA_PAWTUCKET_VERSION=1.7.16


# CLONE PAWTUCKET
RUN rm -rf ${WEB_DOCUMENT_ROOT}/*
RUN git clone --depth 1 --branch ${CA_PAWTUCKET_VERSION} https://github.com/collectiveaccess/pawtucket2.git ${CA_PAWTUCKET_DIR}
# alternative (using tar.gz file extraction):
# RUN curl -SsL https://github.com/collectiveaccess/pawtucket2/archive/refs/tags/$CA_PAWTUCKET_VERSION.tar.gz | tar -C /var/www/ -xzf -
# RUN mv /var/www/pawtucket2-${CA_PAWTUCKET_VERSION}/* ${CA_PAWTUCKET_DIR}



# --------------------------------------------------------------------------------
# "ca-providence-base" stage installs the providence application
FROM ca-pawtucket-base as ca-providence-base

ENV CA_PROVIDENCE_DIR=${WEB_DOCUMENT_ROOT}/providence
ENV CA_PROVIDENCE_VERSION="dev/graphql-api"
# ENV CA_PROVIDENCE_VERSION=1.7.17


# CLONE PROVIDENCE
RUN git clone --depth 1 --branch ${CA_PROVIDENCE_VERSION} https://github.com/collectiveaccess/providence.git ${CA_PROVIDENCE_DIR}
# alternative (using tar.gz file extraction):
# RUN curl -SsL https://github.com/collectiveaccess/providence/archive/refs/tags/${CA_PROVIDENCE_VERSION}.tar.gz | tar -C /var/www/ -xzf -
# RUN mv /var/www/providence-${CA_PROVIDENCE_VERSION} ${CA_PROVIDENCE_DIR}

RUN mkdir -p ${CA_PROVIDENCE_DIR}/app/tmp/collectiveaccessCache && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/images && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/tilepics && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/workspace && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/flv && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/quicktime && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/windowsmedia && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/swf && \
    mkdir -p ${CA_PROVIDENCE_DIR}/media/collectiveaccess/mp3

# PAWTUCKET (link to media folder)
RUN ln -s ${CA_PROVIDENCE_DIR}/media ${CA_PAWTUCKET_DIR}/media


RUN chown -R ${APACHE_RUN_USER}:${APACHE_RUN_GROUP} ${WEB_DOCUMENT_ROOT}



# --------------------------------------------------------------------------------
# "ca-backend" is the final build stage
FROM ca-providence-base as ca-backend

COPY ./.docker/providence/apache2/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ./.docker/providence/apache2/php.ini /etc/php/7.4/apache2/php.ini

# COPY ./.docker/providence/entrypoint.sh /entrypoint.sh
# RUN chmod 777 /entrypoint.sh
# ENTRYPOINT ["/entrypoint.sh"]

CMD [ "/usr/sbin/apache2", "-DFOREGROUND" ]
