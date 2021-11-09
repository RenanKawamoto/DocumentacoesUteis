# **Sintaxe básica do linux alpine**

## *Sumário:*
1. [Apt do alpine](#apt)
    1. [Instalar pacotes](#add)
    2. [Deletando pacotes](#del)
    3. [Vendo atualizações dos pacotes](#update)
    4. [Atualizando os pacotes já existente](#upgrade)
    5. [Buscar pacotes](#search)

Nessa documentação tentarei mostrar os comandos que são diferentes no alpine para outras distros como ubuntu/debian.

<div id = "apt"><div>

## *`Apt do alpine:`*

<div id = "add"><div>

### **Instalando pacotes:**
Comando no Alpine:
~~~
    apk add <nome-do-pacote>
~~~

No Ubuntu/Debian:
~~~
    apt install <nome-do-pacote>
~~~

<div id = "del"><div>

### **Deletando pacotes:**
Comando no Alpine:
~~~
    apk del <nome-do-pacote>
~~~

No Ubuntu/Debian:
~~~
    apt remove <nome-do-pacote>
~~~

<div id = "update"><div>

### **Vedo se existem atualizações dos pacotes:**
Comando no Alpine:
~~~
    apk update 
~~~

No Ubuntu/Debian:
~~~
    apt update 
~~~

<div id = "upgrade"><div>

### **Atualizando os pacotes existentes:**
Comando no Alpine:
~~~
    apk upgrade
~~~

No Ubuntu/Debian:
~~~
    apt upgrade
~~~

<div id = "search"><div>

### **Pesquisar um pacote:**
Comando no Alpine:
~~~
    apk search <nome-do-pacote>
~~~

No Ubuntu/Debian:
~~~
    apt search <nome-do-pacote>
~~~

