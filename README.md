# Dockerfiles for local wordpress envirnoment

Fork from Docker "Official Image" for wordpress (https://github.com/docker-library/wordpress), with addition of:

* xdebug  
* redis
* some erlier versions of php ( 5.6, 7.0 and 7.1 ) - not included anymore in "Official Image"
* option to define wordpress version in the image ( with build argument )

## Usage
example docker-compose.yml file

```
version: '3'

services:
  <PROJECT_NAME>_apache:
    container_name: <PROJECT_NAME>_apache
    build: 
      context: ~/dockerfiles/wordpress/php7.3/apache/
      # args:
      #   wordpress_version: 5.2.2
      #   wordpress_sha1: 3605bcbe9ea48d714efa59b0eb2d251657e7d5b0
    volumes:
      - ./public:/var/www/html/
    working_dir: /var/www/html/
    environment:
      XDEBUG_CONFIG: profiler_output_name=<PROJECT_NAME>.out.%p
    labels:
      - "traefik.http.routers.<PROJECT_NAME>.rule=Host(`<PROJECT_NAME>.localhost`)"
    networks:
      - local_proxy

networks:
  local_proxy:
    external: true
