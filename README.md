# hesk-docker

HESK help desk, but in a Docker container.

HESK version: 3.4.3 from 8th April 2023

## Deployment

To deploy HESK and MariaDB, use the following Docker Compose YAML:

```
---
version: "3"
services:
  db:
    image: mariadb:11.1
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MARIADB_RANDOM_ROOT_PASSWORD: yes
      MARIADB_DATABASE: hesk
      MARIADB_USER: hesk
      # Please change the password before deploying.
      MARIADB_PASSWORD: hesk
    
  hesk:
    depends_on:
      - db
    image: ghcr.io/kimmknight/hesk:latest
    ports:
      - "80:80"
    restart: always
volumes:
  db_data: {}
...
```

You should change the password before deploying in production.

You may also wish to change the external port number from 80.
For example: `8001:80`

## Post-installation setup

Open your web browser to http://yourserverip/install

Follow the steps to install. When you are required to enter database settings, use the following:

| Setting | Value |
| ------- | ----- |
| Database Host | db |
| Database Name | hesk |
| Database User (login) | hesk |
| User Password | hesk |
| Table prefix | hesk_ |

Change *User Password* if you set a different password during deployment.

Proceed through any installation steps.

Open a console into the hesk-docker container, and delete the installation folder from HESK: `rm -r /srv/install`

HESK should now be ready to use at: http://yourserverip/

## About

This is a fork of https://github.com/luketainton/hesk-docker
I have updated HESK and simplified the repo.

##

**Support HESK by [buying a licence](https://www.hesk.com/buy.php).**
