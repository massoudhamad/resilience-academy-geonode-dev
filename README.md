# Resilienceacademy

## Table of Contents

-  [Setup](#Setup)
-  [Build](#Build)

GeoNode-Project Template for `resilienceacademy`

## Setup

1. Clone the git repository.

    ```bash
    cd /opt
    git clone https://github.com/geosolutions-it/resilienceacademy.git
    ```

2. Move to repo folder.

    ```bash
    cd /opt/resilienceacademy
    ```

3. Customize the environment.

    a) `django.env`

      ```bash
      nano scripts/docker/env/production/django.env
      ```

      ```diff
      --- a/scripts/docker/env/production/django.env
      +++ b/scripts/docker/env/production/django.env
      @@ -3,20 +3,20 @@ GEONODE_INSTANCE_NAME=geonode
      DOCKER_ENV=production
      UWSGI_CMD=uwsgi --ini /usr/src/resilienceacademy/uwsgi.ini
      
      -SITEURL=https://localhost/
      +SITEURL=https://<public host>/
      ALLOWED_HOSTS=['django', '*']
      
      -GEONODE_LB_HOST_IP=localhost
      +GEONODE_LB_HOST_IP=<public host>
      # port where the server can be reached on HTTP
      # GEONODE_LB_PORT=80
      # port where the server can be reached on HTTPS
      GEONODE_LB_PORT=443
      
      -ADMIN_PASSWORD=admin
      +ADMIN_PASSWORD=<your admin pwd>
      -ADMIN_EMAIL=admin@gmail.com
      +ADMIN_EMAIL=<your admin email>
      
      -GEOSERVER_WEB_UI_LOCATION=http://localhost/geoserver/
      -GEOSERVER_PUBLIC_LOCATION=http://localhost/geoserver/
      +GEOSERVER_WEB_UI_LOCATION=https://<public host>/geoserver/
      +GEOSERVER_PUBLIC_LOCATION=https://<public host>/geoserver/
      GEOSERVER_LOCATION=http://geoserver:8080/geoserver/
      
      @@ -39,7 +39,7 @@ MOSAIC_ENABLED=False
      BROKER_URL=amqp://guest:guest@rabbitmq:5672/
      MONITORING_ENABLED=True
      MODIFY_TOPICCATEGORY=True
      -
      +AVATAR_GRAVATAR_SSL=True
      EXIF_ENABLED=False
      CREATE_LAYER=False
      FAVORITE_ENABLED=False
      ```

    b) `geoserver.env`

      ```bash
      nano scripts/docker/env/production/geoserver.env
      ```

      ```diff
      --- a/scripts/docker/env/production/geoserver.env
      +++ b/scripts/docker/env/production/geoserver.env
      @@ -1,6 +1,6 @@
      DOCKER_HOST_IP
      
      -GEONODE_LB_HOST_IP=localhost
      +GEONODE_LB_HOST_IP=<public host>
      # port where the server can be reached on HTTP
      # GEONODE_LB_PORT=80
      # port where the server can be reached on HTTPS
      ```

    c) `nginx.env`

      ```bash
      nano scripts/docker/env/production/nginx.env
      ```

      ```diff
      --- a/scripts/docker/env/production/nginx.env
      +++ b/scripts/docker/env/production/nginx.env
      @@ -1,12 +1,12 @@
      
      -ADMIN_EMAIL=admin@gmail.com
      +ADMIN_EMAIL=<your admin email>
      
      # IP or domain name and port where the server can be reached on HTTP (leave HOST empty if you want to use HTTPS only)
      HTTP_HOST=
      HTTP_PORT=80
      
      # IP or domain name and port where the server can be reached on HTTPS (leave HOST empty if you want to use HTTP only)
      -HTTPS_HOST=localhost
      +HTTPS_HOST=<public host>
      HTTPS_PORT=443
      
      # Let's Encrypt certificates for https encryption. You must have a domain name as HTTPS_HOST (doesn't work
      @@ -15,6 +15,6 @@ HTTPS_PORT=443
      # staging : we get staging certificates (are invalid, but allow to test the process completely and have much higher limit rates)
      # production : we get a normal certificate (default)
      #LETSENCRYPT_MODE=disabled
      -LETSENCRYPT_MODE=staging
      +LETSENCRYPT_MODE=production
      
      RESOLVER=127.0.0.11
      ```

## Build

1. Double check `docker-compose.yml` and `docker-compose.override.yml` are correctly configured.

2. Run the docker compose build.


    ```bash
    cd /opt/resilienceacademy
    ./docker-build-sh
    ```

3. Follow the logs in order to be sure the container started up correctly.

    ```bash
    docker-compose logs -f django
    ```

    Wait until you see:

    ```bash
    ...
    django4resilienceacademy | got command ${cmd}
    django4resilienceacademy | [uWSGI] getting INI configuration from /usr/src/resilienceacademy/uwsgi.ini
    ```

4. Check the GeoNode has correctly started up.

    ```bash
    # Browse to
    https://<public host>/
    ```
