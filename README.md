# Plex video streaming server
Docker image for plex video streaming server.

Container runs as non-root user.
This user owns plex process and owns plex files.

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

# Donation
If you find my code useful, you can [bye me a coffee](https://www.paypal.me/dshapovalov)
