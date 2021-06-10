# Plex video streaming server
Docker image for plex video streaming server.

Container runs as non-root user.
This user owns plex process and owns plex files.

To run container next volumes should be mapped:
* folder for configs
* folder for metadata database
* folder for media files
* logs folder
* backups folder

## Installation
### Installation from docker image
1. Pull docker image.
2. Create user and group to own plex files and to run docker container:
    ```
    sudo groupadd -g 961 plex
    ```
    ```
    useradd -u 961 -g 961 -M plex
    ```
3. Proceed to configuration.

### Installation from source
1. Pull project sources from version control system.
2. Create user and group to own plex files and to run docker container:
    ```
    sudo useradd -r plex
    ```
3. Make **build** executable:
    ```
    sudo chmod u+x ./build
    ```
4. Execute **build**:
    ```
    sudo ./build plex
    ```
5. Proceed to configuration.

### Configuration
1. Create folders for plex files:
    ```
    sudo mkdir /plex
    ```
    ```
    sudo mkdir /plex/media
    ```
    ```
    sudo mkdir /plex/media/movies
    ```
    ```
    sudo mkdir /plex/media/series
    ```
    ```
    sudo mkdir /plex/config
    ```
    ```
    sudo mkdir /plex/config/codecs
    ```
    ```
    sudo mkdir /plex/config/plug-in-support
    ```
    ```
    sudo mkdir /plex/config/plug-ins
    ```
    ```
    sudo touch /plex/config/preferences.xml
    ```
2. Create folder for logs:
    ```
    sudo mkdir /var/log/plex
    ```
3. Create folder for backups:
    ```
    sudo mkdir /var/backups/plex
    ```
4. Grant permit to all folders:
    ```
    sudo chown -R plex:plex /plex
    ```
    ```
    sudo chown plex:plex /var/log/plex
    ```
    ```
    sudo chown plex:plex /var/backups/plex
    ```
5. Copy **etc/init.d/plex** to **/etc/init.d** folder:
    ```
    sudo cp ./etc/init.d/plex /etc/init.d
    ```
6. Copy **usr/sbin/plex** to **/usr/sbin** folder:
    ```
    sudo cp ./usr/sbin/plex /usr/sbin
    ```
7. Copy **usr/bin/pxutil** to **/usr/bin** folder:
    ```
    sudo cp ./usr/bin/pxutil /usr/bin
    ```
8. Make all files executable:
    ```
    sudo chmod a+x /etc/init.d/plex
    ```
    ```
    sudo chmod a+x /usr/sbin/plex
    ```
    ```
    sudo chmod a+x /usr/bin/pxutil
    ```
9. Register service:
    ```
    sudo update-rc.d plex defaults
    ```
12. Start plex service:
    ```
    sudo service plex start
    ```

# Donation
If you find my code useful, you can [bye me a coffee](https://www.paypal.me/dshapovalov)
