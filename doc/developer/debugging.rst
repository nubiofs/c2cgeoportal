.. _developer_debugging:

Debugging
=========

The goal of this document is to give some troubleshooting tips.

General
-------

First you should add ``?debug`` in the application URL to have all the
JavaScript and stylesheets in separated non-minified files.

Using a browser-integrated debugging tool such as
`Firebug <https://addons.mozilla.org/fr/firefox/addon/firebug/>`_ (for Firefox)
or Chrome development tool is very useful.

In case of 500 errors have a look at the apache logs, located at
``/var/www/vhosts/<vhost_name>/logs/<vhost_name>_error.log``. If we call a service behind
a proxy, the log entry actually references the final URL. Therefore you may
call the latter URL directly on the server by typing
``curl "http://localhost/<path>" -H Host:<server_name>``.

If the ``pyramid_debugtoolbar`` is enabled the error is directly returned
in the query that fails.

For print-related issues have a look at the logs in the
``/srv/tomcat/tomcat1/logs/<instanceid>.log`` file.

Mapserver
---------

Sometime more information are available by using this command:

.. prompt:: bash

    shp2img -m <mapfile> -o test.png -e <minx> <miny> <maxx> <maxy> -s <sizex> <sizey> -l <layers>

You may also activate Mapserver's debug mode and set the log level in your
mapfile by adding the following configuration::

    MAP
        ...
        CONFIG "MS_ERRORFILE" "/tmp/ms_error.txt"
        DEBUG 5
        ...

`More informations <http://mapserver.org/optimization/debugging.html?highlight=debug#step-2-set-the-debug-level>`_

PostgreSQL
----------

In the ``/etc/postgresql/9.*/main/postgresql.conf`` configuration file
you can set ``log_statement`` to ``all`` to see all the called statements.
This file must be edited using the ``postgres`` user.

Reloading PostgreSQL is required so that the new configuration is taken into
account:

.. prompt:: bash

    sudo /etc/init.d/postgres reload

Logs are available in the ``/var/log/postgresql/postgresql-9.*-main.log`` file.

Makefile
--------

You can run `DEBUG=TRUE make ...` to have some debug message.
Actually we display the running rule and why she is running (dependence update).

Performance or network error
----------------------------

For performance and proxy issues make sure that all internal URLs in the config file
use localhost (use ``curl "http://localhost/<path>" -H Host:<server_name>``
to test it).

Tilecloud chain
...............

Points to check with TileCloud chain::

 * Disabling metatiles should be avoided.
 * Make sure that``empty_metatile_detection`` and ``empty_tile_detection`` are configured correctly.
 * Make sure to not generate tiles with a higher resolution than in the raster sources.
