# Configuração do php-fpm

## php-fpm.conf

~~~
    [global]

    pid = /run/php7-fpm.pid
    error_log = /dev/stderr

    emergency_restart_threshold = 10
    emergency_restart_interval = 1m
    process_control_timeout = 10s

    include = /etc/php7/php-fpm.d/*.conf
~~~

## www.conf (fica em /etc/php7/php-fpm.d/)

~~~
    
~~~