FROM        debian:buster
MAINTAINER  Dmitry Shapovalov

ARG         hostuid=961
ARG         hostgid=961

ENV         DEBIAN_FRONTEND=noninteractive \
            RUN_USER=plex \
            RUN_GROUP=plex \
            PLEX_MEDIA_SERVER_CONFIG_DIR="/var/lib/plexmediaserver" \
            PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR="/var/lib/plexmediaserver/Library/Application Support" \
            PLEX_MEDIA_SERVER_DATA_DIR="/var/lib/plexmediaserver/Library/Application Support/Plex Media Server" \
            PLEX_MEDIA_SERVER_HOME="/usr/lib/plexmediaserver" \
            PLEX_MEDIA_SERVER_MAX_PLUGIN_PROCS=6 \
            PLEX_MEDIA_SERVER_TMPDIR="/tmp" \
            PLEX_MEDIA_SERVER_USER=plex \
            PLEX_MEDIA_SERVER_INFO_VENDOR="Docker" \
            PLEX_MEDIA_SERVER_INFO_DEVICE="Docker Container" \
            LD_LIBRARY_PATH="/usr/lib/plexmediaserver"

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
            apt-get update && \
            apt-get install --assume-yes \
                beignet-opencl-icd \
                jq \
                libusb-1.0-0 \
                locales \
                locales-all \
                ocl-icd-libopencl1 \
                tzdata \
                udev \
                uuid-runtime \
                xmlstarlet && \
            apt-get clean

# Create user and group
RUN         groupadd -g $hostgid $RUN_GROUP && \
            useradd -u $hostuid -g $hostgid -M $RUN_USER

# Configure application folders
RUN         mkdir /plex && \
            mkdir /plex/config && \
            mkdir /plex/config/Cache && \
            mkdir /plex/config/Codecs && \
            mkdir /plex/config/Crash\ Reports && \
            mkdir /plex/config/Media && \
            mkdir /plex/config/Metadata && \
            mkdir /plex/config/Plug-in\ Support && \
            mkdir /plex/config/Plug-ins && \
            touch /plex/config/Preferences.xml && \
            mkdir /plex/media && \
            mkdir /plex/scripts && \
            mkdir /plex/logs && \
            mkdir /plex/backups && \
            chown -R $RUN_USER:$RUN_GROUP /plex

# Configure app
RUN         curl https://downloads.plex.tv/plex-keys/PlexSign.key | apt-key add - && \
            echo "deb https://downloads.plex.tv/repo/deb public main" >> /etc/apt/sources.list.d/plex.list && \
            apt-get update && \
            apt-get install --assume-yes \
                plexmediaserver && \
            apt-get clean && \
            mkdir -p "$PLEX_MEDIA_SERVER_CONFIG_DIR" && \
            mkdir -p "$PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR" && \
            mkdir -p "$PLEX_MEDIA_SERVER_DATA_DIR" && \
            ln -s /plex/config/Codecs "$PLEX_MEDIA_SERVER_DATA_DIR/Codecs" && \
            ln -s /plex/config/Plug-in\ Support "$PLEX_MEDIA_SERVER_DATA_DIR/Plug-in Support" && \
            ln -s /plex/config/Plug-ins "$PLEX_MEDIA_SERVER_DATA_DIR/Plug-ins" && \
            ln -s /plex/config/Preferences.xml "$PLEX_MEDIA_SERVER_DATA_DIR/Preferences.xml" && \
            ln -s /plex/logs "$PLEX_MEDIA_SERVER_DATA_DIR/Logs" && \
            chown -R -h $RUN_USER:$RUN_GROUP "$PLEX_MEDIA_SERVER_CONFIG_DIR"

# Configure helper scripts
RUN         mv /tmp/containerConfig /plex/scripts && \
            mv /tmp/containerDummy /plex/scripts && \
            mv /tmp/containerStartup /plex/scripts && \
            mv /tmp/containerShutdown /plex/scripts && \
            mv /tmp/webStartup /plex/scripts && \
            mv /tmp/webShutdown /plex/scripts && \
            mv /tmp/utilBackup /plex/scripts && \
            mv /tmp/utilRestore /plex/scripts && \
            chmod 755 /plex/scripts/*

USER        $RUN_USER

EXPOSE      1900/udp \
            3005/tcp \
            5353/udp \
            8324/tcp \
            32400/tcp \
            32410/udp \
            32412/udp \
            32413/udp \
            32414/udp \
            32469/tcp

ENTRYPOINT  ["/plex/scripts/containerStartup"]
