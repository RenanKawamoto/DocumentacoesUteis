# **Sintaxe básica Composer**

## *Sumário:*

## *Como instalar:*

- Linux: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04-pt

- Windows: ...

- Mac: ...

## *O que é o composer:*

O composer é um gerenciador de dependências do PHP. Ele permite que você declare as bibliotecas que seu projeto necessita e ele gerencia (instala / atualiza) elas para você.

## *Onde procurar as dependencias:*

https://packagist.org/

## *Comandos básicos:*

- `composer init` ===> irá gerar um arquivo composer.json no diretorio atual(que é a configuração do seu projeto) e uma pasta vendor (contem as bibliotecas instaladas). 

- `composer require <nomeDaDependencia>` ===> irá instalar mais uma dependencia no seu composer.json.

- `composer update` ===> atualiza todas as alterações feitas no composer.json.

- `composer install` ===> irá ver as linhas require do composer.json, e intalar as que ainda não foram instaladas.

## *Como chamar todas as depencias para um arquivo php:*

~~~php
    require_once '<path_vendor>/autoload.php'
~~~

