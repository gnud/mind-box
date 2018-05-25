# Moodle Initial setup

```bash
git clone https://github.com/moodlehq/moodle-docker
cd moodle-docker

# Development Moodle
git clone https://github.com/moodle/moodle apps

# Export env variables
export MOODLE_DOCKER_WWWROOT=$(pwd)/apps/

# Choose a db server (Currently supported: pgsql, mariadb, mysql, mssql, oracle)
export MOODLE_DOCKER_DB=pgsql

cp config.docker-template.php $MOODLE_DOCKER_WWWROOT/config.php

# Start up containers
bin/moodle-docker-compose up -d
```

If you closely notice, it resembles the 'Quick Start', however, we added the Moodle download as well.
Alternatively, if you prefer the stable version, you can just download and extract the [stable version](https://download.moodle.org/releases/latest/)
by replacing the 'Development Moodle' with 'Stable Moodle' step.

```bash
# Stable Moodle
wget https://download.moodle.org/download.php/stable35/moodle-latest-35.tgz
tar xf moodle-latest-35.tgz --one-top-level=apps --strip-components=1
```


More info at [See Quick Start](https://github.com/moodlehq/moodle-docker)

Inspect the containers:

Available containers:
- moodlehq/moodle-php-apache:7.1 - provides php and apache binaries
- moodledocker_webserver_1 - this hosts the /var/www
- mailhog/mailhog - fake SMTP server with web inteface for an inbox
- selenium/standalone-firefox:2.53.1
- postgres:9.6.7 - the db

This drops into the shell, allowing to inspect the setup.
```bash
docker exec -i -t moodledocker_webserver_1 /bin/bash
```

# Email container
Edit file service.mail.yml in section mailhog

```yaml
  mailhog:
    image: mailhog/mailhog
    ports:
      - "8025:8025"
```
Make sure you add the ports section, set the left side as you desire, but make sure you use the same in your browser.

## Restart server
```bash
bin/moodle-docker-compose down
bin/moodle-docker-compose up
```

## Check email in your browser
Visit URL http://localhost:8025/

# Moodle web
Visit URL http://localhost:8000/
This will open the installation wizard, to complete an initial admin setup.
Now visit before that in a new tab prepare the [email url](#check-email-in-your-browser), after the user creation is complete
you will see a new mail in the inbox.

## Connect Mozilla's backpack API
Create an account [here](https://backpack.openbadges.org/), set the backpack email in your settings. Then a mail will arrive, so check [inbox](#check-email-in-your-browser).
Open the mail an click verify, when you reload the Moodle's preference page, you will see it's verified.

**Note**: Make sure you set the backpack collection
as public at openbadges.org, so that Moodle can manage it.
