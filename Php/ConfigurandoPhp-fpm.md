# Configuração do php-fpm

## php-fpm.conf
Essa configuração fica em /etc/php7/php-fpm.conf.
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
    [www]
    catch_workers_output = yes
    group = nginx
    listen = 127.0.0.1:9000;
    listen.allowed_clients = 127.0.0.1
    listen.backlog = -1
    listen.group = nginx
    listen.mode = 0666
    listen.owner = nginx
    pm = dynamic
    pm.max_children = 15
    pm.mas_requests = 200
    pm.max_spare_servers = 4
    pm.min_spare_servers = 2
    pm.start_servers = 3
    pm.status_path = /status
    request_slowlog_timeout = 15s
    request_terminate_timeout = 120s
    rlimit_core = unlimited
    rlimit_files = 131072
    slowlog = /dev/stdout
    user = nginx
~~~


## **Para tirar as duvidas:**
https://pt.stackoverflow.com/questions/207464/como-funciona-o-php-fpm