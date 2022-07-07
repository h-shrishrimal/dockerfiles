FROM ubuntu:latest

LABEL Author="Harshad S <hshrishrimal@gmail.com>"
LABEL org.opencontainers.image.authors="Harshad S <hshrishrimal@gmail.com>"
LABEL version="1.0"

#
# default arguments
#
ARG USERNAME=harshad
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG APP_DIRECTORY=/app
ARG NODE_VERSION=16

#
# application will run on port
#
EXPOSE 4200

#
# Create the user
#
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} 

#
# Create the directory
#
VOLUME [ "${APP_DIRECTORY}" ]

#
# Change ownership
#
RUN chown -R ${USERNAME}:${USER_GID} ${APP_DIRECTORY}

#
# Install curl and other build essentials
#
RUN apt-get update \
    && apt-get install -y curl gnupg build-essential \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get remove -y --purge cmdtest \
    && apt-get update \
    # remove useless files from the current layer
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/lib/apt/lists.d/* \
    && apt-get autoremove \
    && apt-get clean \
    && apt-get autoclean

#
# Install node and angular cli
#
RUN curl --silent --location https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash -  \
    && apt-get install -y nodejs \
    && npm install --location=global @angular/cli  

#
# Change user
#
USER ${USERNAME}

#
# Setup the working directory
#
WORKDIR ${APP_DIRECTORY}

CMD ["/bin/bash"]