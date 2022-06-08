# Plex video streaming server
Docker image for plex video streaming server.

Container runs as non-root user.
This user owns plex process and owns plex files.

To run container next volumes should be mapped:
* folder for configs
* folder for metadata database
* folder for media files
* folder to upload files
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
    sudo mkdir /plex/config
    ```
    ```
    sudo mkdir /plex/config/cache
    ```
    ```
    sudo mkdir /plex/config/codecs
    ```
    ```
    sudo mkdir /plex/config/crash-reports
    ```
    ```
    sudo mkdir /plex/config/media
    ```
    ```
    sudo mkdir /plex/config/metadata
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
    ```
    sudo mkdir /plex/media
    ```
    ```
    sudo mkdir /plex/media/ad_movies
    ```
    ```
    sudo mkdir /plex/media/ad_series
    ```
    ```
    sudo mkdir /plex/media/ch_movies
    ```
    ```
    sudo mkdir /plex/media/ch_series
    ```
    ```
    sudo mkdir /plex/media/ed_movies
    ```
    ```
    sudo mkdir /plex/media/ed_series
    ```
    ```
    sudo mkdir /plex/upload
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
    sudo chmod 777 /plex/upload
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
13. Obtain content of the **Preferences.xml**
    ```
    sudo pxutil showPreferences
    ```
    And write this content to the **/plex/config/preferences.xml** file.

## Management
### Service management
```
sudo service plex (start|stop|status|restart)
```

### Create backup
```
sudo pxutil backup <filename>
```

Backup file **/var/backups/plex/&lt;filename&gt;.tar.gz** will be created.

### Restore backup
```
sudo pxutil restore <filename>
```

### Command line (bash)
```
sudo pxutil bash
```

## Apache mod_proxy configuration
Plex video streaming server can be located with another web applications.
For example, mercurial, bugzilla, wiki etc can be run as docker containers on the same host.
In this case apache server can be used to redirect requests to different docker containers.

1. Enable apache mod_proxy:
    ```
    sudo a2enmod deflate headers proxy proxy_ajp proxy_balancer proxy_connect proxy_html proxy_http rewrite
    ```
2. Configure proxy:
    ```
    <VirtualHost *:80>
        ...
        ProxyPreserveHost On
        <Proxy *>
            Order allow,deny
            Allow from all
        </Proxy>
        ...
    </VirtualHost>
    ```
3. Copy **./etc/apache2/sites-available/plex.conf** to **/etc/apache2/sites-available** folder:
    ```
    sudo cp ./etc/apache2/sites-available/plex.conf /etc/apache2/sites-available
    ```
4. Enable apache sites:
    ```
    sudo a2ensite plex
    ```
5. Restart apache service:
    ```
    sudo service apache2 restart
    ```

## VPN configuration
Plex uses external resources (for example, the Movie Database) that can be not accessible without VPN.
To make them accessible VPN should be used.
Container contains OpenVPN client, that can be used to turn the VPN connection on.

1. Set values for these environment variables in **/usr/sbin/plex** file:
    * VPN_USERNAME
    * VPN_PASSWORD
2. Get the list of available configurations:
    ```
    sudo pxutil vpnList
    ```
3. Turn the VPN connection on with one of these configurations:
    ```
    sudo pxutil vpnStartup <configuration>
    ```

When there is no need in VPN anymore, turn the VPN connection off.

2. Turn the VPN connection off:
    ```
    sudo pxutil vpnShutdown
    ```

## HOW TO
### How to create cron job for backups
```
sudo crontab -l | { cat; echo "minute hour * * * /usr/bin/pxutil backup <filename>"; echo ""; } | sudo crontab -
```

# Donation
If you find my code useful, you can [bye me a coffee](https://www.paypal.me/dshapovalov)
