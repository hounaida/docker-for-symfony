docker-symfony
==============

This is a complete stack for running Symfony 4 (latest version: Flex) into Docker containers using docker-compose tool.

**Note:** Docker must be already installed in your machine :)

# Installation

1. Clone this repository:

    ```bash
    $ git clone git@github.com:hounaida/docker-for-symfony.git
    ```

3. Put your Symfony application into `symfony` folder and do not forget to add `symfony.localhost` in your `/etc/hosts` file.

    ```bash
    $ cd docker-for-symfony
    $ git clone git@bitbucket.org:clubouttn/social-web-app.git symfony
    ```

4. Make sure you adjust `DATABASE_URL` in `symfony/.env` file like the following:

    ```yaml
    $ DATABASE_URL=mysql://root:symfony@db/symfony
    ```

5. Build all docker images

    ```bash
    $ docker-compose build
    ```
    
5. Run containers

    ```bash
    $ docker-compose up -d
    ```
    
6. Install project dependencies

    ```bash
    $ docker exec -it docker-for-symfony_php_1 /bin/sh
    $ composer install
    ```
    
7. Create database schema & populate sample data

    ```bash
    $ docker exec -it docker-for-symfony_php_1 /bin/sh
    $ php bin/console doctrine:schema:create
    $ bin/console doctrine:fixtures:load
    ```

You are done, you can visit your Symfony application on the following URL: `http://symfony.localhost/`.  
You can also visit PhpMyAdmin via this URL: `http://symfony.localhost:8080/`

# How it works?

Here are the `docker-compose` built images:

* `db`: This is the MySQL database container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too,
* `phpmyadmin`: This is the phpmyadmin container.

This results in the following running containers:

```bash
> $ docker-compose ps
        Name                       Command               State              Ports
--------------------------------------------------------------------------------------------
dockersymfony_db_1      docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp
dockersymfony_nginx_1   nginx                            Up      443/tcp, 0.0.0.0:80->80/tcp
dockersymfony_php_1     php-fpm7 -F                      Up      0.0.0.0:9000->9000/tcpdockersymfony_php_1
phpmyadmin/phpmyadmin   /run.sh supervisord              Up      9000/tcp, 0.0.0.0:8080->80/tcp
```

# Read logs

You can access Nginx and Symfony application logs in the following directories on your host machine:

* `logs/nginx`
* `logs/symfony`
