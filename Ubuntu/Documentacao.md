# **Guia básico de Linux Ubuntu**

## *Sumário:*
1. [Atalhos uteis do Ubuntu](#atalhos)
2. [Comandos do terminal](#terminal)
    1. [Relacionados ao diretorio](#dir)
        1. [Saber usuario atual](#whoami)
        2. [Saber o diretorio atual](#pwd)
        3. [Limpar tela do terminal](#clear)
        4. [Listar conteudo do diretorio atual](#ls)
        5. [Listar conteudo e permissões do diretorio atual](#lsl)
        6. [Listar conteudo explicito e conteudo oculto do diretorio atual](#lsa)
        7. [Entrar dentro de um diretorio](#cd)
        8. [Sair de um diretorio](#cdp)
        9. [Voltar para o diretorio HOME](#cdhome)
        10. [Criar um diretorio/pasta](#mkdir)
        11. [Deletar um diretorio/pasta vazio(a)](#rmdir)
        12. [Criar um arquivo](#touch)
        13. [Deletar um arquivo](#rm)
        14. [Deletar diretorio/pasta com conteudo](#rmr)
    2. [APT](#apt)
        1. [Mudar para super usuario](#sudo);
        2. [Verificar atualizações nos repositorios do sistema](#update)
        3. [Verificar quais atualizações nos pacotes podem ser feitas](#upgradable)
        4. [Procurar se um pacote já está intalado pelo nome](#list)
        5. [Procurar pacote dentro dos repositorios](#search)
        6. [Instalar um pacote](#install)
        7. [Desinstalar um pacote](#remove)
        8. [Instalar e atualizar os pacotes que você já possui](#upgrade)
        9. [Remover, instalar e atualizar os pacotes que você já possui](#fullupgrade)
        10. [Mostrar mais informações sobre um pacote já intalado](#show)
        11. [Mostrar quais são as dependencias de um pacote já instalado](#depends)
    3. [DPKG](#dpkg)
        1. [Instalar um pacote/programa com dpkg](#dpkg)
        2. [Desinstalar um pacote/programa com dpkg](#dpkgr)
    4. [PPA](#ppa)
3. [Diretorios do Linux](#dirl)
    1. [Indicações visuais do diretorio](#dirlv)
    2. [Diretorios/pastas padrões do linux](#pastas)
        1. [principal](#principal)
        2. [bin](#bin)
        3. [boot](#boot)
        4. [cdrom](#cdrom)
        5. [dev](#dev)
        6. [etc](#etc)
        7. [home](#home)
        8. [lib, lib32, lib64, libx32](#lib)
        9. [media](#media)
        10. [mnt](#mnt)
        11. [opt](#opt)
        12. [proc](#proc)
        13. [root](#root)
        14. [run](#run)
        15. [sbin](#sbin)
        16. [snap](#snap)
        17. [srv](#srv)
        18. [sys](#sys)
        19. [tmp](#tmp)
        20. [usr](#usr)
        21. [var](#var)

<div id = "atalhos"></div>

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

<div id = "terminal"></div>


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

<div id = "dir"></div>


## *-Comandos relacionados aos diretorios:*

<div id = "whoami"></div>

### **Identificar seu usuario atual:**
~~~
    whoami
~~~

<div id = "pwd"></div>

### **Mostrar em que diretorio/pasta você está:**
~~~
    pwd
~~~

<div id = "clear"></div>

### **Limpar o terminal:**
~~~
    clear
~~~

*OBS: caso não queira limpar com o comando é possível fazer isso através do atralho ctrl + l;*

<div id = "ls"></div>

###  **Listar tudo que está dentro do diretório atual:**
~~~
    ls
~~~

OBS: Esse comando apresenta algumas variações com flags.

<div id = "lsl"></div>

### **Ls que mostra toda a estrutura de permissões:**
~~~
    ls -l
~~~

<div id = "lsa"></div>

### **Ls que mostra todos os arquivos ocultos:**
~~~
    ls -A
~~~

<div id = "cd"></div>

### **Mudar de diretorio:**
~~~
    cd <nomeDoDiretoriQueDejaIr>
~~~

<div id = "cdp"></div>

### **Voltar um diretorio:**
~~~
    cd ..
~~~

<div id = "cdhome"></div>

### **Voltar para o diretorio HOME:**
~~~
    cd ~
~~~ 

<div id = "mkdir"></div>

### **Criar um diretorio/pasta:**
~~~
    mkdir nomeDaPasta
~~~
OBS: caso você queira criar uma pasta com espaços utilize as aspas simples( ' ). Porém não é uma boa prática.
~~~
    mkdir 'nome da pasta'
~~~

<div id = "rmdir"></div>

### **Remover um diretorio/pasta vazio(a):**
~~~
    rmdir nomeDaPasta
~~~

~~~
    rmdir 'nome da pasta'
~~~

<div id = "touch"></div>

### **Criar um arquivo (TOUCH):**
~~~
    touch nomeDoArquivo.Extensao
~~~

<div id = "rm"></div>

### **Remover arquivos (RM):**
~~~
    rm nomeDoArquivo.Extensao
~~~

<div id = "rmr"></div>

### **Remover um diretorio/pasta com conteudo:**
~~~
    rm -r nomeDaPasta
~~~
OBS: o `-r` é uma flag que tem a função de indicar para o rm, remover recursivamente, assim excluindo o conteudo da pasta e depois a propria pasta.

<div id = "apt"></div>

## *-Comando relacionados a instalação com apt:*

<div id = "sudo"></div>

### **Mudar para o super usuario:**
~~~
    sudo su
~~~

<div id = "update"></div>

### **Verificar atualizações na lista de repositorios do sistema:**
~~~
    sudo apt update
~~~
* Nesse caso o sudo é para se você não estiver como super usuario, ele significa super usuario faça (super user do).

* apt(significa advanced package tool) é um conjunto de ferramentas usadas pelo GNU/Linux Debian e suas respectivas derivações, entre eles o Ubuntu, para administrar os pacotes .deb de uma forma automática.

<div id = "upgradable"></div>

### **Listar quais updates podem ser feitos, baseado do apt update:**
~~~
    apt list --upgradable
~~~

<div id = "list"></div>

### **Lista os pacotes que você tem instalado baseado no nome:**

Sintaxe:
~~~
    apt list nomeDoPacote
~~~

Exemplo:
~~~
    apt list gimp
~~~

<div id = "search"></div>

### **Procurar se existe um pacote detro dos seus repositorios que você deseja intalar:**

Sintaxe:
~~~
   apt search nomeDoPacote 
~~~

Exemplo:
~~~
    apt search inkscape
~~~

<div id = "install"></div>

### **Instalar um pacote:**

Sintaxe:
~~~
    apt install nomeDoPacote
~~~

Exemplo:
~~~
    apt install inkscape
~~~

<div id = "remove"></div>

### **Desinstalar um pacote:**

Sintaxe:
~~~
    sudo apt remove nomeDoPacote
~~~

Exemplo:
~~~
   sudo apt remove inkscape 
~~~

<div id = "upgrade"></div>

### **Instalar e a atualizar os pacotes segundo o apt update:**

~~~
   sudo apt upgrade 
~~~

<div id = "fullupgrade"></div>

### **Remover, instalar e a atualizar os pacotes segundo o apt update:**

~~~
   sudo apt full-upgrade 
~~~

<div id = "show"></div>

### **Mostrar informações mais detalhadas sobre os pacotes já instaldos:**

Sintaxe:
~~~
    apt show nomeDoPacote
~~~

Exemplo:
~~~
    apt show gimp
~~~

<div id = "depends"></div>

### **Ver quais são as dependencias de um pacote:**

Sintaxe:
~~~
    apt-cache depends nomeDoPacote
~~~

Exemplo:
~~~
    apt-cache depends inkscape
~~~

<div id = "dpkg"></div>

## *-Comandos de instalação com o DPKG:*

Existem situações em que o pacote que você busca não existe no apt, sendo assim é necessário instala-lo utilizando DPKG, para isso vá no site do software e baixe o arquivo.deb.

Após isso vá até o diretorio onde o arquivo se encontra e utilize o seguinte comando:

~~~
    sudo dpkg -i nomeDoArquivo.deb
~~~
* O dpkg é um utilitario para instalar arquivos com extensão .deb;
* `-i` é uma flag que tem a função de indicar que você deseja fazer a instalação.

<div id = "dpkgr"></div>

### **Desinstalando um pacote com o DPKG:**
~~~
    sudo dpkg -r nomeDoPacote
~~~

<div id = "ppa"></div>

## *-Comando de instalação com o PPA:*

Ainda irei pesquisar sobre.

---------------------------------------

<div id = "dirl"></div>

# *- Entendendo os diretorios do linux:*

<div id = "dirlv"></div>

## *-Indicações visuais dos diretorios:*

### **Pasta com uma seta:**
As pastas que possuem essa seta, são chamadas de link simbolico, que são como atalhos para outras pastas ou arquivos.

![linux_img1](https://uploaddeimagens.com.br/images/003/186/999/full/linux_img1.png?1617891559)

### **Pasta com um X:**
As pastas/arquivos que apresentam um X são aquelas(es) que você, com seu usuario atual não tem acesso, sendo assim você pode acessa-las(los) com o root caso queira.

![linux_img2](https://uploaddeimagens.com.br/images/003/187/000/full/linux_img2.png?1617891585)

<div id = "pastas"></div>

## *-Pastas padrões do Ubuntu:*
*OBS: para acessa-las é necessário entrar em Arquivo >> outros locais >> partiçãoQueDesejaAcessar.*

<div id = "principal"></div>

### **Diretorio padrão/principal( / ):**
É o diretorio onde serão encontrados todos os diretorios listados a seguir:

<div id = "bin"></div>

### **Diretorio bin:**
Esse diretorio se referencia a "binario", sendo assim ele contem os executaveis de diversos programas do sistema operacional, links simbolicos e shell scripts(são rotinas de comando, muito semelhante aos .bat).

<div id = "boot"></div>

### **Diretorio boot:**
`CUIDADO`:
Esse diretorio contem os arquivos necessários para o seu sistema operacional inicializar.

<div id = "cdrom"></div>

### **Diretorio cdrom:**
A imagem dos discos (CD/DVD) serão montadas nessa pasta.

<div id = "dev"></div>

### **Diretorio dev:**
Dev significado device, sendo assim é um local onde você encontrara os arquivos que correspondem ao seu hardware.

<div id = "etc"></div>

### **Diretorio etc:**
Corresponde a um diretorio onde serão encontrados as configurações do sistema de maneira "global" (para todos o usuarios).

<div id = "home"></div>

### **Diretorio home:**
Aqui ficam pastas dos usuarios cadastrados nessa maquina.

<div id = "lib"></div>

### **Diretorio lib, lib32, lib64, libx32:**
Esses diretorios armazenam as bibliotecas necessárias para o funcionamento de aplicações.

* lib: armazena as bibliotecas mutiarquitetura;
* lib32: armazena as bibliotecas 32bits;
* lib64: armazena as bibliotecas 64bits;
* libx32: armazena as bibliotecas x32ABI;

<div id = "media"></div>

### **Diretorio media:**
Nesse diretorio é onde irão ser montadas as unidades removiveis do seu sistema.

<div id = "mnt"></div>

### **Diretorio mnt:**
Nesse diretorio é onde serão montadas as unidades feitas manualmente pelo usuario.

<div id = "opt"></div>

### **Diretorio opt:**
Nesse diretorio é onde serão encontrados softwares que desejam organizar seus arquivos em um unico local.

<div id = "proc"></div>

### **Diretorio proc:**
Nesse diretorio é onde serão encontrados arquivos que contem informações sobre o sistema e processos dele. ELE É UM `DIRETORIO VIRTUAL`, SENDO ASSIM ESSES ARQUIVOS NÃO ESTÃO REALMENTE NESSA PASTA E SÃO CARREGADOS NOVAMENTE TODA VEZ QUE O SISTEMA REINICIA.

<div id = "root"></div>

### **Diretorio root:**
É tipo o home do root.

<div id = "run"></div>

### **Diretorio run:**
OUTRO DIRETORIO `VIRTUAL`

Ele armazena as informações desde o ultimo boot.

<div id = "sbin"></div>

### **Diretorio sbin:**
Ele é semelhante ao bin, assim armazenando executaveis e links simbolicos, porém apenas dos programas que necessitam do super usuario.

<div id = "snap"></div>

### **Diretorio snap:**
Contem os arquivos do snap.

<div id = "srv"></div>

### **Diretorio srv:**
Abreviação de services.

Se você estiver rodando algum servidor web ou servidor ftp, você pode armazenar aqui os arquivos que são acessiveis aos outros usuarios.

<div id = "sys"></div>

### **Diretorio sys:**
OUTRO DIRETORIO `VIRTUAL`

É um diretorio que tem função de você interegir com o kernel linux(O Kernel Linux é um núcleo monolítico de código aberto para sistemas operacionais tipo UNIX).

<div id = "tmp"></div>

### **Diretorio tmp:**
É um diretorio que armazena os arquivos temporario gerados pelos aplicativos (esses devem ser apagados durante cada reboot).

<div id = "usr"></div>

### **Diretorio usr:**
Aqui você encontra arquivos e programas uteis para os usuarios, porém que não são essencias para o funcionamento básico do sistema.

<div id = "var"></div>

### **Diretorio var:**
Aqui são armazenados arquivos que são esperados que aumentem de tamanho ao longo do tempo.

