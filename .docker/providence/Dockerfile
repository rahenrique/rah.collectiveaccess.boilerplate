FROM webdevops/php-apache:7.4

ENV WEB_DOCUMENT_ROOT=/var/www/html
ENV CA_PROVIDENCE_VERSION=1.7.14
ENV CA_PROVIDENCE_DIR=${WEB_DOCUMENT_ROOT}/providence
ENV CA_PAWTUCKET_VERSION=1.7.14
ENV CA_PAWTUCKET_DIR=${WEB_DOCUMENT_ROOT}

ENV PHP_MEMORY_LIMIT=2048M
ENV PHP_MAX_EXECUTION_TIME=1200
ENV PHP_POST_MAX_SIZE=50M
ENV PHP_UPLOAD_MAX_FILESIZE=50M
ENV PHP_DISPLAY_ERRORS=1
ENV PHP_DATE_TIMEZONE=America/Sao_Paulo

RUN apt-get update && apt-get install -y \
    ffmpeg \
    dcraw \
    ghostscript \
    imagemagick \
    libreoffice


USER 1000:1000
WORKDIR ${WEB_DOCUMENT_ROOT}


# PAWTUCKET
RUN rm -rf ${WEB_DOCUMENT_ROOT}/*

RUN git clone --depth 1 --branch ${CA_PAWTUCKET_VERSION} https://github.com/collectiveaccess/pawtucket2.git ${CA_PAWTUCKET_DIR}
# RUN composer install -o -d ${CA_PAWTUCKET_DIR}


# PROVIDENCE
RUN git clone --depth 1 --branch ${CA_PROVIDENCE_VERSION} https://github.com/collectiveaccess/providence.git ${CA_PROVIDENCE_DIR}
# RUN curl -SsL https://github.com/collectiveaccess/providence/archive/refs/tags/${CA_PROVIDENCE_VERSION}.tar.gz | tar -xzf -
# RUN mv ./providence-${CA_PROVIDENCE_VERSION} ${CA_PROVIDENCE_DIR}
# RUN rm -rf ${CA_PROVIDENCE_DIR}/providence-${CA_PROVIDENCE_VERSION}
# RUN composer install -o -d ${CA_PROVIDENCE_DIR}

RUN mkdir ${CA_PROVIDENCE_DIR}/app/tmp/collectiveaccessCache && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/images && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/tilepics && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/workspace && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/flv && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/quicktime && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/windowsmedia && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/swf && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess/mp3


# # PAWTUCKET (link to media folder)
RUN ln -s ${CA_PROVIDENCE_DIR}/media ${CA_PAWTUCKET_DIR}/media

# # This is the default entrypoint location for the webdevops images
# # https://dockerfile.readthedocs.io/en/latest/content/Customization/provisioning.html
# # COPY ./.docker/providence/entrypoint.sh /opt/docker/provision/entrypoint.d/99-userscript.sh
