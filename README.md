gudron.php_fpm_pool
===================

Role for creating php-fpm pools config files

Role Variables
--------------

### General variables

  * `default_pools_params: dict`
    Default pool parameters. Like `user`, `group`, `request_terminate_timeout` and etc.

    * `user: string`
      Pool worker operation system user. [Official php-fpm documentation](https://www.php.net/manual/ru/install.fpm.configuration.php#user).

    * `group: string`
      Pool worker operation ystem group. [Official php-fpm documentation](https://www.php.net/manual/ru/install.fpm.configuration.php#group).

    * `pm: dict`
      Pool process manager parameters.

      * `strategy: string`
        Strategy for creating pool processes. [Official php-fpm documentation](https://www.php.net/manual/ru/install.fpm.configuration.php#pm).

    Supported variables: [defaults/main/ssl.yml](defaults/main/ssl.yml).

Dependencies
------------


Example Playbook
----------------

License
-------

Apache 2.0