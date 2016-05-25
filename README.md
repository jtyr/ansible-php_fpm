php_fpm
=======

Ansible role which helps to install and configure PHP-FPM.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
---

# Example of how to install PHP-FPM
- hosts: host1
  roles:
    - php_fpm

# Example of how to create custom PHP-FPM pool
- hosts: host2
  vars:
    php_fpm_pool_config:
      # This is the pool file name
      zabbix:
        # This is the content of the file
        zabbix:
          listen: /var/run/php-fpm/php-zabbix.socket
          listen.owner: root
          listen.group: apache
          listen.mode: 0660
          user: nginx
          group: nginx
          pm: dynamic
          pm.max_children: 50
          pm.start_servers: 5
          pm.min_spare_servers: 5
          pm.max_spare_servers: 35
          slowlog: /var/log/php-fpm/zabbix-slow.log
          catch_workers_output: yes
          security.limit_extensions: .php .php3 .php4 .php5
          php_admin_value[error_log]: /var/log/php-fpm/zabbix-error.log
          php_admin_flag[log_errors]: on
          php_value[session.save_handler]: files
          php_value[session.save_path]:  /var/lib/php/session/
  roles:
    - php_fpm
```


Role variables
--------------

Variables used by the role is as follows:

```
# Name of the PHP-FPM service
php_fpm_service: php-fpm

# Package to be installed (explicit version can be specified here)
php_fpm_pkg: php-fpm

# Path to the php-fpm.conf file
php_fpm_config_file: /etc/php-fpm.conf

# Path to the php-fpm.d directory
php_fpm_config_dir: /etc/php-fpm.d

# Default PHP-FPM config (not processed if set to null)
php_fpm_config: null
# Example:
#php_fpm_config:
#  include: /etc/php-fpm.d/*.conf
#  global:
#    pid: /var/run/php-fpm/php-fpm.pid
#    error_log: /var/log/php-fpm/error.log
#    daemonize: no

# Default polls config
php_fpm_pool_config: {}
# Example:
#php_fpm_pool_config:
#  # This is the file name
#  zabbix:
#    # This is the content of the file
#    zabbix:
#      listen: /var/run/php-fpm/php-zabbix.socket
#      listen.owner: root
#      listen.group: apache
#      listen.mode: 0660
#      user: nginx
#      group: nginx
#      pm: dynamic
#      pm.max_children: 50
#      pm.start_servers: 5
#      pm.min_spare_servers: 5
#      pm.max_spare_servers: 35
#      slowlog: /var/log/php-fpm/zabbix-slow.log
#      catch_workers_output: yes
#      security.limit_extensions: .php .php3 .php4 .php5
#      php_admin_value[error_log]: /var/log/php-fpm/zabbix-error.log
#      php_admin_flag[log_errors]: on
#      php_value[session.save_handler]: files
#      php_value[session.save_path]:  /var/lib/php/session/

# Path to the sysconfig file
php_fpm_sysconfig_file: /etc/sysconfig/php-fpm

# Sysconfig configuration
php_fpm_sysconfig: {}
# Example:
#php_fpm_sysconfig:
#  pidfile: /var/run/php-fpm/php-fpm.pid
#  lockfile: /var/lock/subsys/php-fpm
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)
- [`php`](https://github.com/jtyr/ansible-php) role (optional)


License
-------

MIT


Author
------

Jiri Tyr
