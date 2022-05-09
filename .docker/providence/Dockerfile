FROM webdevops/php-nginx:7.4

ENV WEB_DOCUMENT_ROOT=/var/www/html/providence
ENV PHP_MEMORY_LIMIT=2048M
ENV PHP_MAX_EXECUTION_TIME=1200
ENV PHP_POST_MAX_SIZE=50M
ENV PHP_UPLOAD_MAX_FILESIZE=50M
ENV PHP_DISPLAY_ERRORS=1

ENV CA_PROVIDENCE_VERSION=1.7.14
ENV CA_PROVIDENCE_DIR=${WEB_DOCUMENT_ROOT}
ENV CA_PAWTUCKET_VERSION=1.7.14
ENV CA_PAWTUCKET_DIR=${WEB_DOCUMENT_ROOT}/pawtucket

# RUN apt-get update && apt-get install -y \
#     # default-mysql-client \
#     ffmpeg \
#     ghostscript \
#     imagemagick \
#     libreoffice

WORKDIR ${CA_PROVIDENCE_DIR}


# PROVIDENCE
RUN git clone --depth 1 --branch ${CA_PROVIDENCE_VERSION} https://github.com/collectiveaccess/providence.git ${CA_PROVIDENCE_DIR}

RUN mkdir ${CA_PROVIDENCE_DIR}/app/tmp/collectiveaccessCache && \
    mkdir ${CA_PROVIDENCE_DIR}/media/collectiveaccess

RUN chmod -R 777 ${CA_PROVIDENCE_DIR}/app/tmp && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/app/log && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache && \
    chmod -R 777 ${CA_PROVIDENCE_DIR}/media


# PAWTUCKET
RUN git clone --depth 1 --branch ${CA_PAWTUCKET_VERSION} https://github.com/collectiveaccess/pawtucket2.git ${CA_PAWTUCKET_DIR}
# # RUN mv ${CA_PAWTUCKET_TEMP_DIR}/* ${CA_PAWTUCKET_DIR}
RUN ln -s ${CA_PROVIDENCE_DIR}/media ${CA_PAWTUCKET_DIR}/media

RUN chmod -R 777 ${CA_PAWTUCKET_DIR}/app/tmp && \
    chmod -R 777 ${CA_PAWTUCKET_DIR}/app/log && \
    chmod -R 777 ${CA_PAWTUCKET_DIR}/vendor/ezyang/htmlpurifier/library/HTMLPurifier/DefinitionCache


# PERMISSIONS FOR WEB USER
# RUN chown -R www-data:www-data ${WEB_DOCUMENT_ROOT}

# This is the default entrypoint location for the webdevops images
# https://dockerfile.readthedocs.io/en/latest/content/Customization/provisioning.html
# COPY ./.docker/providence/entrypoint.sh /opt/docker/provision/entrypoint.d/99-userscript.sh
