*******************************
CS-Cart Development Environment
*******************************

.. contents::
   :local:

==============
English manual
==============

Docker-based development environment:

* PHP versions: 8.2, 8.1, 8.0 and 7.4.
* MySQL 5.7 database server.
* nginx web server.

------------
Installation
------------

#. Install ``git``, ``docker`` and ``docker-compose``.
#. Clone the environment repository:

    .. code-block:: bash

        $ git clone git@github.com:cscart/development-docker.git ~/srv
        $ cd ~/srv

#. Create the directory to store CS-Cart files:

    .. code-block:: bash

        $ mkdir -p app/www

#. Clone CS-Cart repository or unpack the distribution archive into the ``app/www`` directory.
#. Enable the default application config for nginx:

    .. code-block:: bash

        $ cp config/nginx/app.conf.example config/nginx/app.conf

#. Run application containers:

    .. code-block:: bash

        $ make -f Makefile run

----------------
MySQL connection
----------------
        
* DB host: mysql5.7.
* User: root.
* Password: root. 


-----------------------------------
Working with different PHP versions
-----------------------------------

PHP 7.4 is used by default.

To use the specific PHP version for your requests, add the following prefix to the domain you request:

* ``php7.4.`` for PHP 7.4.
* ``php8.0.`` for PHP 8.0.
* ``php8.1.`` for PHP 8.1.
* ``php8.2.`` for PHP 8.2.

---------------
Sending e-mails
---------------

PHP containers do not send actual e-mails when using the ``mail()`` function.

All sent emails will be caught and stored in the ``app/log/sendmail`` directory.

----------------------------------
Working with multiple applications
----------------------------------

See comments in the ``config/nginx/app.conf.example`` file if you need to host multiple PHP applications inside single Docker PHP container.

----------------------------------
Enabling xDebug for PHP containers
----------------------------------

xDebug 3 is already configured for PHP7 and PHP8 containers. All you have to do is to uncomment the extension installation in the ``config/php*/Dockerfile`` files.

You can read about configuring PHPStorm to work with Docker and xDebug 3 in the `"Debugging PHP" <https://thecodingmachine.io/configuring-xdebug-phpstorm-docker>`_ article.

------------------------
Configuring the Docker subnet
------------------------

Docker-compose creates a subnet with addresses by default 172.18.[0-255].[0-255].

If you run docker locally with a default subnet, then resources using the same addresses will be unavailable - the response will be returned by the local subnet, not the required resource.

To fix the problem, you need to change the address of the docker subnet.

In the docker-compose file.bml shows an example of replacing addresses with 10.10.[0-255].[0-255].

Uncomment the lines in docker-compose.yml and run the following commands:

    .. code-block:: bash

        $ docker network rm $(docker network ls -q)
        $ docker-compose down && docker-compose up -d