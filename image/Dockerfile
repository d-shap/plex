FROM        debian:buster
MAINTAINER  Dmitry Shapovalov

ARG         hostuid=961
ARG         hostgid=961

ENV         DEBIAN_FRONTEND=noninteractive \
            RUN_USER=plex \
            RUN_GROUP=plex

COPY        . /tmp

# Install common packages and configure environment
RUN         apt-get update && \
            apt-get install --assume-yes \
                apt-utils \
                build-essential \
                cron \
                curl \
                net-tools \
                procps \
                software-properties-common \
                vim \
                wget && \
            apt-get clean

# Create user and group
RUN         groupadd -g $hostgid $RUN_GROUP && \
            useradd -u $hostuid -g $hostgid -M $RUN_USER

# Configure application folders
RUN         mkdir /plex && \
            mkdir /plex/media && \
            mkdir /plex/logs && \
            mkdir /plex/backups && \
            chown -R $RUN_USER:$RUN_GROUP /plex

# Configure app
RUN         curl https://downloads.plex.tv/plex-keys/PlexSign.key | apt-key add - && \
            echo "deb https://downloads.plex.tv/repo/deb public main" >> /etc/apt/sources.list.d/plex.list && \
            apt-get update && \
            apt-get install --assume-yes \
                plexmediaserver && \
            apt-get clean
