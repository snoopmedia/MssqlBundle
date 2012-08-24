# Configuration #

**1.** Add the bundle to your deps file:

    #deps:
    [MssqlBundle]
        git=https://github.com/snoopmedia/MssqlBundle.git
        target=/bundles/Realestate/MssqlBundle

**2.** Update your vendors:

    % bin/vendors install

**3.** Include the namespace in your autoloader.php:

    #app/autoloader.php
    $loader->registerNamespaces(array(
        [...]
        'Realestate'       => __DIR__.'/../vendor/bundles',
        [...]
    ));

**4.** Make the driver_class configurable by the parameters.ini:

    #app/config/config.yml:
    doctrine:
        dbal:
            driver_class: %database_driver_class%
            driver:   %database_driver%

**5.** Configure the driver class by adding it to the parameters.ini:

    #app/config/parameters.ini:
    [parameters]
        database_driver_class = \Realestate\MssqlBundle\Driver\PDODblib\Driver
        database_driver   = pdo_sqlsrv ;this is only a placeholder
        database_host     = mssql-server
        database_port     =
        database_name     = symfony
        database_user     = root
        database_password =


***
This driver requires version 8.0 (from http://www.ubuntitis.com/?p=64) as default 4.2 version does not have UTF support

In /etc/freetds/freetds.conf, change

    tds version = 4.2
to

    tds version = 8.0


***
can't use nvarchar!!


***
In SQL 2000 SP4 or newer, SQL 2005 or SQL 2008, if you do a query that returns NTEXT type data, you may encounter the following exception:

    _mssql.MssqlDatabaseError: SQL Server message 4004, severity 16, state 1, line 1:
    Unicode data in a Unicode-only collation or ntext data cannot be sent to clients using DB-Library (such as ISQL) or ODBC version 3.7 or earlier.

It means that SQL Server is unable to send Unicode data to FTREETDS, because of shortcomings of FTREETDS. You have to CAST or CONVERT the data to equivalent NVARCHAR data type, which does not exhibit this behaviour.
