gudron.php_fpm_pool
===================

Role for creating php-fpm pools config files.

Role Variables
--------------

  * `php_version_param: string`
    The php version for which you want to generate the configuration.

  * `conf_file_path_param: string`
    The path to the directory where you want to save the configuration files.

  * `default_pools_params: dict`
    Default pool parameters. Like `user`, `group`, `request_terminate_timeout` and etc.

    * `user: string`
      Pool worker operation system user. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#user).

    * `group: string`
      Pool worker operation system group. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#group).

    * `port: int`
      Pool worker listen port. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#listen).

    * `pm: dict`
      Pool process manager parameters.

      * `strategy: string`
        Strategy for creating pool processes. Allowed values `static`, `ondemand`, `dynamic`, [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm).

      * `max_children: int`
        Then maximum number of child processes. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm.max-children).

      * `start_servers: int`
        The number of child processes created on startup. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm.start-servers).

      * `min_spare_servers: int`
        The desired minimum number of idle server processes. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm.min-spare-servers).

      * `max_spare_servers: int`
        The desired maximum number of idle server processes. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm.max-spare-servers).

      * `max_requests: int`
        The number of requests each child process should execute before respawning. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#pm.max-requests).

    * `process: dict`
      Dictionary of process parameters.
      
      * `priority: int`
        Specify the nice(2) priority to apply to the worker process (only if set). [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#worker-process-priority).

      * `dumpable: boolean`
        Set the process dumpable flag (PR_SET_DUMPABLE prctl) even if the process user or group is different than the master process user. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#process-dumpable).

    * `security: dict`
      Dictionary of security parameters.

      * `limit_extensions: list`
        List of strings for limits the extensions of the main script FPM will allow to parse. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#security-limit-extensions).

    * `admin_values: dict`
      Dictionary of `php_admin_value` PHP settings. PHP settings passed with `php_admin_value` will overwrite their previous value. Passed keys and values will be renders "as is".

    * `admin_flags: dict`
      Dictionary of `php_admin_flag` PHP settings. PHP settings passed with `php_admin_flag` will overwrite their previous value. Passed keys and values will be renders "as is". Passed values must contains only True or False.

    * `val: dict`
      Dictionary of `php_value` PHP settings. PHP settings passed with `php_value` will overwrite their previous value. Passed keys and values will be renders "as is".

    * `flags: dict`
      Dictionary of `php_flag` PHP settings. PHP settings passed with `php_flag` will overwrite their previous value. Passed keys and values will be renders "as is". Passed values must contains only True or False.

    * `ping: dict`
      Dictionary of ping parameters.

      * `response: string`
        This directive may be used to customize the response to a ping request. The response is formatted as text/plain with a 200 response code. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#ping.response).

    * `request_terminate_timeout: string`
      The timeout for serving a single request after which the worker process will be killed. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#request-terminate-timeout).

    * `request_slowlog_timeout: string`
      The timeout for serving a single request after which a PHP backtrace will be dumped to the `slowlog` file. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#request-slowlog-timeout).

    * `slowlog: string`
      The log file for slow requests. Parameter will be processed only if `request_slowlog_timeout`not a zero. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#slowlog).

    * `rlimit_files: int`
      Set open file descriptor rlimit for child processes in this pool. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#rlimit-files).

    * `clear_env: boolean`
      Clear environment in FPM workers. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#clear-env).

    * `catch_workers_output: boolean`
      Redirect worker stdout and stderr into main error log. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#catch-workers-output).

    * `decorate_workers_output: boolean`
      Enable the output decoration for workers output when `catch_workers_output` is enabled. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#decorate-workers-output).

  * `pools_params: dict`
    Dictionary contains parameters of PHP-FPM pool. By default, parameters from `default_pools_params` are inherited. 

    * `env_variables: list`
      List of environment variables which will be passed to pool process. Passed variables will be rendered if `clear_env` parameter set to True.

  * `disabled_functions_params: list`
    List of default disabled functions. [Official php-fpm documentation](https://www.php.net/manual/en/ini.core.php#ini.disable-functions).

  Supported variables: [defaults/main.yml](defaults/main.yml).

Instalation
-----------

Add **gudron.php_fpm_pool** role to your *requirements* file.

```yaml
  - src: git@github.com:gudron/gudron.php_fpm_pool.git
    scm: git
    version: master
```

Install roles via **ansible-galaxy** tool.

```bash
ansible-galaxy install -p roles -r requirements.yml
```

Example Playbook
----------------

    - hosts: example_project:&example_project_stage
      any_errors_fatal: "{{ any_errors_fatal | default(true) }}"
      gather_facts: false
      vars:
        ansible_connection: local
        ansible_become: no
        ansible_distribution: Debian
            
      roles:
        - name: gudron.php_fpm_pool
          vars: 
            php_version: 7.3
            conf_file_path: /example/project/php-fpm/pool.d/sites-available/
            pools_params:
              example_pool_1:
                port: 9011
                user: example_user
                group: example_user
                slowlog: /path/to/slowlog/exmaple_pool_1.log
                env_variables:
                  - MYSQL_POOL1_WORKER_USER
                  - MYSQL_POOL1_WORKER_PASSWORD

              example_pool_2:
                port: 9012
                user: example_user_2
                group: example_user_2
                security:
                  limit_extensions:
                    - .php
                    - .php7
                env_variables:
                  - MYSQL_POOL2_WORKER_USER
                  - MYSQL_POOL2_WORKER_PASSWORD

License
-------

Apache 2.0