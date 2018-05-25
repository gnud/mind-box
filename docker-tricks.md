**NOTE**: This is a draft document, not tested.

# Start a new platform
create a file named docker-compose.yml

```yaml
version: '2'
services:
```

Now let's think for a second what our platform needs.

Obviosly if we build a web app, we need a database, most times we require memory caching servers as well.
Most importantly we need a web server and email server.


We can split the docker compose yml files into categories, like mail service, web service and etc.

Create a main file called docker-compose.yml

```yaml
version: '2'
services:
  webserver:
    image: "romeoz/docker-apache-php:${DOCKER_PHP_VERSION}"
    depends_on:
      - db
    volumes:
      - "${DOCKER_WWWROOT}:/var/www/html"
      - "${ASSETDIR}/web/apache2_faildumps.conf:/etc/apache2/conf-enabled/apache2_faildumps.conf"
  db:
    image: postgres:9.6.7
    environment:
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: "A2#s$%sfg"
      POSTGRES_DB: myapp
```

create a file named service-mail.yml

```yaml
version: '2'
services:
  webserver:
    volumes:
      - "${ASSETDIR}/web/apache2_mailhog.conf:/etc/apache2/conf-enabled/apache2_mailhog.conf"
    depends_on:
      - mailhog
  mailhog:
    image: mailhog/mailhog
    ports:
      - "8025:8025"
```

create a file named db.mysql.yml

```yaml
version: "2"
services:
  webserver:
    environment:
      DOCKER_DBTYPE: mysqli
      DOCKER_DBCOLLATION: utf8_bin
  db:
    image: mysql:6
    command: >
                --character-set-server=utf8
                --collation-server=utf_bin
                --innodb_file_format=barracuda
                --innodb_file_per_table=On
                --innodb_large_prefix=On
    environment:
      MYSQL_ROOT_PASSWORD: "A2#s$%sfg"
      MYSQL_USER: myuser
      MYSQL_PASSWORD: "A2#s$%sfg"
      MYSQL_DATABASE: myapp

```

More comming soon...
