FROM        debian:buster
MAINTAINER  Dmitry Shapovalov

ARG         hostuid=961
ARG         hostgid=961

ENV         DEBIAN_FRONTEND=noninteractive \
            RUN_USER=plex \
            RUN_GROUP=plex

COPY        . /tmp

# Install common packages and configure environment
RUN         echo "deb https://downloads.plex.tv/repo/deb public main" >> /etc/apt/sources.list.d/plex.list && \
            apt-get update && \
            apt-get install --assume-yes \
                apt-utils \
                build-essential \
                cron \
                net-tools \
                procps \
                software-properties-common \
                vim \
                wget && \
            apt-get update && \
            apt-get install --assume-yes \
                curl && \
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
