# **Documentação básica sobre nginx**

## *Sumário:*

## *O que é?*


## *Como instalar:*

No Linux(Ubuntu):
~~~
    apt install nginx
~~~

No Linux(Alpine):
~~~
    apk add nginx
~~~

No Windows:
...

No Mac: 
...

## *Como dar status/start/stop/reload/restart:*


### **Status:**
Tem a função de mostrar se o nginx está rodando, além de outras informações importantes.

No Ubuntu:
~~~
    systemctl status nginx
~~~

No Alpine:
~~~
    rc-service nginx status
~~~

No Windows:
...

No Mac:
...

### **Start:**
Tem a função de iniciar o nginx.

No Ubuntu:
~~~
    systemctl start nginx
~~~

No Alpine:
~~~
    rc-service nginx start
~~~
OBS: Para que você possa startar o nginx no alpine é necessáio antes, utilizar o comando `rc-status --manual`.

No Windows:
...

No Mac:
...

### **Stop:**
Tem a função de parar o nginx.

No Ubuntu:
~~~
    systemctl stop nginx
~~~

No Alpine:
~~~
    rc-service nginx stop
~~~

No Windows:
...

No Mac:
...

### **Reload:**
Tem a função de atualizar os aquivos do nginx.

No Ubuntu:
~~~
    systemctl reload nginx
~~~

No Alpine:
~~~
    rc-service nginx reload
~~~

No Windows:
...

No Mac:
...

### **Restart:**
Tem a função de reiniciar o nginx.

No Ubuntu:
~~~
    systemctl restart nginx
~~~

No Alpine:
~~~
    rc-service nginx restart
~~~

No Windows:
...

No Mac:
...

## *Arquivos de configuração do nginx:*
Este se encontra em /etc/nginx

### **Nginx.conf:**

Configurações padrões que vem no nginx.conf:
~~~
    user www-data;  
    worker_processes 4;  
    pid /run/nginx.pid;

    events {  
        worker_connections 768;
        # multi_accept on;
    }

    http {

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        gzip on;
        gzip_disable "msie6";

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        include /etc/nginx/conf.d/*.conf;
    }
~~~

* O `user` www-data; significa que o Nginx irá executar como usuário www-data

* O parâmetro `worker_processes` 4; é a espinha dorsal para o processamento do Nginx. Esta diretiva é responsável por deixar o nosso servidor virtual ciênte de que muitos processos são gastos de uma vez.

* O `pid` /run/nginx.pid define um arquivo que irá armazenar o ID de processo do processo principal. 

* A diretiva `events` fornece a configuração em que as diretivas que afetam o processamento de conexão são especificados. Neste caso, o worker_connections 768 define o número máximo de conexões simultâneas que podem ser abertas por um processo de trabalho (768 por padrão).

OBS: Nota: Perceba que ainda dentro de events temos a diretiva multi_accept on; que por padrão vem comentado. Isto é, um processo vai aceitar uma nova conexão de cada vez. Caso contrário, um processo iria aceitar todas as novas conexões ao mesmo tempo

* A diretiva `http` assim como a diretiva events, funcionam como se fossem funções que encapsulam diretivas dentro delas. A diretiva http contém as configurações do protocolo http. Dentro do http temos alguns parâmetros. 

* A diretiva `sendfile on;` em modo on, isto é, ativo, não bloqueia a saída e entrada do disco informando que os dados não estão na memória. Em seguida, o nginx inicia uma carga de dados assíncronos através da leitura de um byte.

*  Diretiva `tcp_nopush on;` só é usada quando o sendfile também está ativo. Pois, esta diretiva é reponsável por enviar o cabeçalho de resposta de pacotes para o sistema operacional.

* A diretiva `tcp_nodelay on;` só pode estar ativa quando um há transferência para o estado keep-alive(Uma conexão Keep-Alive significa uma conexão persistente, Ou uma conexão de vida persistente, entre o cliente e o servidor). 

*  diretiva `keepalive_timeout` define um limite de tempo durante o qual uma conexão de cliente keep-alive vai ficar aberta no servidor. O valor zero desativa conexões keep-alive do cliente. 

* A diretiva `types_hash_max_size` define o tamanho máximo das hash tables. No caso, o padrão é 2048.

* A diretiva `server_tokens off;` que está comentada por padrão, serve para emitir mensagens de erro ao servidor. Esta diretiva é extremamente importante quando não se sabe bem a causa de uma suposta falha envolvendo o nginx.

* A diretiva `server_names_hash_bucket_size;` que também está comentada, define o tamanho de buckets para as tabelas de nome de hash para servidores. O valor padrão depende do tamanho do cache do processador. 

* A diretiva `server_name_in_redirect off` que também está comentada por padrão, é responsável por ativar ou desativar o uso do nome do servidor principal, especificado pela diretiva server_name, em redirecionamentos emitidos pelo nginx.

* A diretiva `include /etc/nginx/mime.types` define o tipo MIME padrão de uma resposta. Isto é, o mapeamento das extensões de nome de arquivo para tipos de MIME pode ser definida com a diretiva types que corresponde a uma extensa lista de formados e extensões permitidas no nginx.

* A diretiva `default_type application/octet-stream;` é uma declaração direta de mime type que não está declarado dentro do arquivo mime.types ativando a aplicação de stream de vídeo octet-stream.

* A diretiva `ssl_protocols` por padrão cria compatibilidade aos formatos ssl.

* Temos também as diretivas para geração de logs em seus devidos caminhos, no caso a diretiva `access_log e error_log` que são gerados por padrão em /var/log/nginx nos arquivos access.log e error.log.

* Temos as diretivas para habilitar formatos de compactação através das configurações do formato gzip, da diretiva `gzip_disable` "msie6" que desabilita a compatibilidade com navegadores Internet Explorer 6 para evitar problemas, e outras configurações pertinentes para este tipo de compabilidade além de mime.types do formato.

* Por fim, em `Virtual Host Configs`, é onde adicionamos os caminhos dos arquivos de configuração contidos na pasta sites-available, conf.d ou qualquer outra pasta que julgue pertinente.


### **Conf.d/**

Cada arquivo dentro deste diretório termina com .conf e é lido na configuração quando Nginx é iniciado garantindo assim, que cada arquivo defina sintaxe válida de configuração do Nginx.

### **Default.conf**

~~~
    server {
        listen [::]:80 default_server;
        listen 80 default_server;

        server_name  localhost;

        charset utf-8;

        access_log  /dev/stdout;
        error_log   /dev/stderr error;

        index index.php index.html index.htm;

        root /www;

        location / {
            root /www;
            try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
            try_files      $uri =404;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
    }
~~~

* `listen [::]:80 default_server; / listen 80 default_server;` ambos comandos tem a função de definir uma porta padrão para o nginx.

*  `server_name  <nome>;` tem a função de nomear seu servidor.

* `charset utf-8;` tem a função de utilizar o padrão utf-8 nos caracteres.

* `access_log  <path>; / error_log <path>;` ambos tem a função de criar logs, sendo um de acessos e outro de erros.

* `index index.php index.html index.htm;` o comando index, tem a função de identificar quais arquivos index.extensão serão interpretados ao bater na url padrão.

* `root /www;` root indica qual será a pasta principal que o nginx irá procurar os arquivos.

* `location /` indica que a url paradrão(localhost/) irá seguir os parametros dentro do bloco.

* `try_files $uri $uri/ /index.php$is_args$args;` tem a função de ao bater na url/ mostrar o arquivo /index.php contendo argumentos.

* `location ~ \.php$ ` indica que todas as urls/algo.php irão seguir os seguintes paramentros.

* `try_files      $uri =404;` tenta chamar a url/nome_do_arquivo.extensão, caso não consiga irá mostrar o erro 404.

* `fastcgi_pass   127.0.0.1:9000;` indica o ip que está rodando o php-fpm.

* `fastcgi_index  index.php;` indica que tenta rodar o index.php primeiro.

* `fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;` são parametros necessários para rodar o fastcgi.

* `include        fastcgi_params;` "importa" os fastcgi_params.

## *Maneira para ver os processos que estão rodando e os historicos de comandos:*

### **Pegando os processos:**
~~~
    ps aux
~~~

### **Vendo os comndos:**
~~~
    history | grep <comando>
~~~