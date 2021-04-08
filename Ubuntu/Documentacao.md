# **Guia básico de Linux Ubuntu**

## *Sumário:*


# *- Atalhos uteis do Ubuntu:*

* **shift + alt + t** = abre o terminal.

* **alt + F2** = abre uma janela para executar um comando.

* **Super(tecla Windows) + shift + Pg_Up/Pg_Dn** = tranfere o aplicativo atual para outra "janela"/área de trabalho.

* **Super(tecla Windows) + Pg_Up/Pg_Dn** = muda entre as "janela"/áreas de trabalho criadas.

* **Super(tecla Windows) + seta_esquerda** = ira ajustar o programa atual para a esquerda.

* **Super(tecla Windows) + seta_direita** = ira ajustar o programa atual para a direita.


* **Super(tecla Windows) + seta_cima** = ira ajustar o programa atual para ocupar a tela inteira.

* **Super(tecla Windows) + seta_baixo** = ira ajustar o programa atual para ocupar quase a tela inteira.

* **Super(tecla Windows)** = vai mostrar todas as "janelas"/áreas de trabalhos que vc possui, atualmente.

------------------------------

# *- Comandos do terminal linux:*

**OBS: Caso você deseje ver o manual de qualquer um dos comando que vem a seguir, você pode utilizar o `man`:**

Sintaxe:
~~~
    man nomeDoComando
~~~

Exemplo:
~~~
    man ls
~~~

## *-Comandos relacionados aos diretorios:*

### **Identificar seu usuario atual:**
~~~
    whoami
~~~

### **Mostrar em que diretorio/pasta você está:**
~~~
    pwd
~~~

### **Limpar o terminal:**
~~~
    clear
~~~

*OBS: caso não queira limpar com o comando é possível fazer isso através do atralho ctrl + l;*

###  **Listar tudo que está dentro do diretório atual:**
~~~
    ls
~~~

OBS: Esse comando apresenta algumas variações com flags.

### **Ls que mostra toda a estrutura de permissões:**
~~~
    ls -l
~~~

### **Ls que mostra todos os arquivos ocultos:**
~~~
    ls -A
~~~

### **Mudar de diretorio:**
~~~
    cd <nomeDoDiretoriQueDejaIr>
~~~

### **Voltar um diretorio:**
~~~
    cd ..
~~~

### **Voltar para o diretorio HOME:**
~~~
    cd ~
~~~ 

### **Criar um diretorio/pasta:**
~~~
    mkdir nomeDaPasta
~~~
OBS: caso você queira criar uma pasta com espaços utilize as aspas simples( ' ). Porém não é uma boa prática.
~~~
    mkdir 'nome da pasta'
~~~

### **Remover um diretorio/pasta vazio(a):**
~~~
    rmdir nomeDaPasta
~~~

~~~
    rmdir 'nome da pasta'
~~~

### **Criar um arquivo (TOUCH):**
~~~
    touch nomeDoArquivo.Extensao
~~~

### **Remover qualquer coisa (RM):**
~~~
    rm nomeDoArquivo.Extensao
~~~

### **Remover um diretorio/pasta com conteudo:**
~~~
    rm -r nomeDaPasta
~~~
OBS: o `-r` é uma flag que tem a função de indicar para o rm, remover recursivamente, assim excluindo o conteudo da pasta e depois a propria pasta.

## *-Comando relacionados a instalação com apt:*

### **Mudar para o super usuario:**
~~~
    sudo su
~~~

### **Verifica atualizações na lista de repositorios do sistema:**
~~~
    sudo apt update
~~~
* Nesse caso o sudo é para se você não estiver como super usuario, ele significa super usuario faça (super user do).

* apt(significa advanced package tool) é um conjunto de ferramentas usadas pelo GNU/Linux Debian e suas respectivas derivações, entre eles o Ubuntu, para administrar os pacotes .deb de uma forma automática.

### **Listar quais updates podem ser feitos, baseado do apt update:**
~~~
    apt list --upgradable
~~~

### **Lista os pacotes que você tem instalado baseado no nome:**

Sintaxe:
~~~
    apt list nomeDoPacote
~~~

Exemplo:
~~~
    apt list gimp
~~~

### **Procurar se existe um pacote detro dos seus repositorios que você deseja intalar:**

Sintaxe:
~~~
   apt search nomeDoPacote 
~~~

Exemplo:
~~~
    apt search inkscape
~~~

### **Instalar um pacote:**

Sintaxe:
~~~
    apt install nomeDoPacote
~~~

Exemplo:
~~~
    apt install inkscape
~~~

### **Desinstalar um pacote:**

Sintaxe:
~~~
    sudo apt remove nomeDoPacote
~~~

Exemplo:
~~~
   sudo apt remove inkscape 
~~~

### **Instalar e a atualizar os pacotes segundo o apt update:**

~~~
   sudo apt upgrade 
~~~

### **Remover, instalar e a atualizar os pacotes segundo o apt update:**

~~~
   sudo apt full-upgrade 
~~~

### **Mostrar informações mais detalhadas sobre os pacotes já instaldos:**

Sintaxe:
~~~
    apt show nomeDoPacote
~~~

Exemplo:
~~~
    apt show gimp
~~~

### **Ver quais são as dependencias de um pacote:**

Sintaxe:
~~~
    apt-cache depends nomeDoPacote
~~~

Exemplo:
~~~
    apt-cache depends inkscape
~~~

## *-Comandos de instalação com o DPKG:*

Existem situações em que o pacote que você busca não existe no apt, sendo assim é necessário instala-lo utilizando DPKG, para isso vá no site do software e baixe o arquivo.deb.

Após isso vá até o diretorio onde o arquivo se encontra e utilize o seguinte comando:

~~~
    sudo dpkg -i nomeDoArquivo.deb
~~~
* O dpkg é um utilitario para instalar arquivos com extensão .deb;
* `-i` é uma flag que tem a função de indicar que você deseja fazer a instalação.

### **Desinstalando um pacote com o DPKG:**
~~~
    sudo dpkg -r nomeDoPacote
~~~

## *-Comando de instalação com o PPA:*

Ainda irei pesquisar sobre.

---------------------------------------

# *- Entendendo os diretorios do linux:*

## *-Indicações visuais dos diretorios:*

### **Pasta com uma seta:**
As pastas que possuem essa seta, são chamadas de link simbolico, que são como atalhos para outras pastas ou arquivos.

![linux_img1](https://uploaddeimagens.com.br/images/003/186/908/full/linux_img1?1617889925)

### **Pasta com um X:**
As pastas/arquivos que apresentam um X são aquelas(es) que você, com seu usuario atual não tem acesso, sendo assim você pode acessa-las(los) com o root caso queira.

![linux_img2](https://uploaddeimagens.com.br/images/003/186/919/full/linux_img2?1617890292)

## *-Pastas padrões do Ubuntu:*
*OBS: para acessa-las é necessário entrar em Arquivo >> outros locais >> partiçãoQueDesejaAcessar.*

