# Dolibarr on Docker

Docker image for Dolibarr with auto installer on first boot.

## Supported tags

* `4.0.0`, `4.0.1`, `4.0.2`, `4.0.3`, `4.0.4`, `latest`

## What is Dolibarr ?

Dolibarr ERP & CRM is a modern software package to manage your organization's activity (contacts, suppliers, invoices, orders, stocks, agenda, ...).

> [More informations](https://github.com/dolibarr/dolibarr)

## How to run this image ?

This image is based on the [officiel PHP repository](https://registry.hub.docker.com/_/php/).

**Important**: This image don't contains database. So you need to link it with a database container.

Let's use [Docker Compose](https://docs.docker.com/compose/) to integrate it with [MariaDB](https://hub.docker.com/_/mariadb/) (you can also use [MySQL](https://hub.docker.com/_/mysql/) if you prefer).

Create `docker-compose.yml` file as following:

```
mariadb:
    image: mariadb:latest
    environment:
        MYSQL_ROOT_PASSWORD: root
        MYSQL_DATABASE: dolibarr

web:
    image: tuxgasy/dolibarr
    environment:
        DOLI_DB_HOST: mariadb
        DOLI_DB_USER: root
        DOLI_DB_PASSWORD: root
        DOLI_DB_NAME: dolibarr
        DOLI_URL_ROOT: 'http://0.0.0.0'
        PHP_INI_DATE_TIMEZONE: 'Europe/Paris'
    ports:
        - "80:80"
    links:
        - mariadb
```

Then run all services `docker-compose up -d`. Now, go to http://0.0.0.0 to access to the new Dolibarr installation.

## Environment variables summary

| Variable                      | Default value      | Description |
| ----------------------------- | ------------------ | ----------- |
| **DOLI_DB_HOST**              | *mysql*            | Host name of the MariaDB/MySQL server
| **DOLI_DB_USER**              | *doli*             | Database user
| **DOLI_DB_PASSWORD**          | *doli_pass*        | Database user's password
| **DOLI_DB_NAME**              | *dolidb*           | Database name
| **DOLI_ADMIN_LOGIN**          | *admin*            | Admin's login create on the first boot
| **DOLI_ADMON_PASSWORD**       | *admin*            | Admin'password
| **DOLI_URL_ROOT**             | *http://localhost* | Url root of the Dolibarr installation
| **PHP_INI_DATE_TIMEZONE**     | *UTC*              | Default timezone on PHP