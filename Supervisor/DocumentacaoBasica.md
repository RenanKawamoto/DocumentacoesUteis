# **Documentação básica supervisor:**

## *SUMÁRIO:*

## *O que é?*
É uma ferramenta de monitoração que controla vários processos filhos e controla o início/reinício desses processos filhos quando eles encerram abruptamente ou encerram por alguma razão

## *Como configurar o supervisor:*
Em primeiro lugar, precisamos de um arquivo de configuração do Supervisor. Supervisor, por padrão, procura uma pasta etc/conf.d que tenha um arquivo chamado supervisord.conf.

**Arquivo Supervisord.conf**
~~~
    [supervisord] #nome do serviço/processo
    nodaemon=true
    [include]
    files = /nomeDaPasta/*
~~~

* `nodaemon=true` tem a função de iniciar o supervisor em primeiro plano (antes dos serviços).

* `files = /nomeDaPasta/*` inclue todo o conteudo da pasta nesse arquivo.

**Arquivo incluido ao supervisord.conf**

**- nginx.conf**
~~~
    [program:nginx]
    command=nginx -g 'daemon off;'
    autostart=true
    autorestart=true
    stdout_logfile=/dev/stdout
    stdout_logfile_maxbytes=0
    stderr_logfile=/dev/stderr
    stderr_logfile_maxbytes=0
~~~

* `[program:nginx]` indica o nome do serviço

* `command=` tudo após essa termologia é um comando que será executado.

* `stdout_logfile=/dev/stdout / stdout_logfile_maxbytes=0` indica o diretorio e a quantidade maxima bytes para armazenar o log de saida.

* `stderr_logfile=/dev/stderr / stderr_logfile_maxbytes=0` indica o diretorio e a quantidade maxima bytes para armazenar o log de erro.

**- php-fpm.conf**
~~~
    [program:php-fpm]
    command=php-fpm7 -F
    autostart=true
    autorestart=true
    stdout_logfile=/dev/stdout
    stdout_logfile_maxbytes=0
    stderr_logfile=/dev/stderr
    stderr_logfile_maxbytes=0
~~~





