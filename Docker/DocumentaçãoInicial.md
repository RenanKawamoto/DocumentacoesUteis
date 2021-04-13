# **Documentação básica sobre o Docker**

## *`Sumário:`*
1.
2.
3.

## *`O que é?`*
É uma ferramenta de empacotamento de uma aplicação e suas dependências em um container virtual que pode ser executado em um servidor linux.

* Ambiente de execução auto-contido;
* Kernel(é o componente central do sistema operativo da maioria dos computadores; ele serve de ponte entre aplicativos e o processamento real de dados feito a nível de hardware) compartilhado com o Host;
* Isolamento dos demais containeres;
* Baixo overhead e tempo de boot.

### **Docker é igual a uma máquina virtual?**
-Não. O Docker tem como principio os container, e esses possuem uma arquitetura diferente, que permite maior portabilidade e eficiência.

## *`Instalando o docker:`*

### **No Linux (Ubuntu 20.04):**

-https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt

### **No Windows:**
-...

### **No Mac:**
-...


## *`O que é uma imagem?`*
Uma imagem é um modelo/template/"ISO"/"VDI" somente de leitura, que é utilizado como base para subir um container.

## *`Subindo seus primeiros containeres:`*

### **Subindo, apenas uma imagem:**

Sintaxe:
~~~
    sudo docker run --name <NomeDoContainer> <Imagem>:<versão>
~~~

Exemplo:
~~~
    sudo docker run --name containerTeste1 Ubuntu:14.04
~~~

### **Subindo um container e dando um comando:**

Sintaxe:
~~~
    sudo docker run --name <NomeDoContainer> <Imagem>:<versão> <path> <comando>
~~~

Exemplo:
~~~
    sudo docker run --name containerTeste2 Ubuntu:14.04 /bin/echo 'Ola mundo'
~~~

`OBS: Após os exemplos citados, é possível notar que os cotaineres não ficam em pé, pois "Containeres são executados enquanto o comando especificado está ativo"`

### **Subindo um container no "modo iterativo":**

Sintaxe:
~~~
    sudo docker run -t -i --name <NomeDoContainer> <Imagem>:<versão> <path> <comando>
~~~

Exemplo:
~~~
    sudo docker run -t -i --name containerTeste3 Ubuntu:14.04 /bin/bash 
~~~

### **Subindo um container no modo daemon:**

Sintaxe:
~~~
    sudo docker run -d --name <NomeDoContainer> <Imagem>:<versão> <path> <comando>
~~~

Exemplo:
~~~
    sudo docker run -d --name containerTeste4 Ubuntu:14.04 /bin/bash -c "while true; do echo ola mundo; sleep 1; done"
~~~

## *`Entendendo principais comandos e flags:`*

### **Mostrar a versão do seu docker:**
~~~
    docker version
~~~
*OBS: Esse comando irá mostrar a versão do docker tanto do cliente quando do servidor*

### **Listar as imagens que temos no nosso host:**
~~~
    docker images
~~~

### **Procurar imagens compativeis com nosso servidor:**
Sintaxe:
~~~
    docker search <nomeDaImagem>
~~~

Exemplo:
~~~
    docker search node
~~~

### **Baixando imagem para o nosso host:**

Sintaxe:
~~~
    docker pull <nomeDaImagem>
~~~

Exemplo:
~~~
    docker pull node
~~~

### **Criando um container (Hello world)**

Sintaxe:
~~~
    docker run <nomeDaImagem>
~~~

Exemplo:
~~~
    docker run hello-world
~~~

*OBS: Caso você não possua a imagem em questão no seu host, ele irá até o repositório central para baixa-la*

### **Verificando o status dos nossos conteineres em execução:**
~~~
    docker ps
~~~

### **Verificando o status de TODOS os nossos conteineres:**
~~~
    docker ps -a
~~~

### **Retornar informações sobre um determinado container:**
Sintaxe:
~~~
    docker stats <id_Ou_apelido_do_container>
~~~

Exemplo:
~~~
    docker stats 4fd993a164db
~~~

### **Retornar mais detalhes sobre uma determinada imagem ou o seu container:**
Sintaxe:
~~~
    docker inspect <id_Da_imagem_ou_container>
~~~

Exemplo:
~~~
    docker inspect 4fd993a164db
~~~
OBS: Ele irá retornar um json com todas as informações relacionadas a nossa busca.

### **Deletando uma imagem do seu host:**

Sintaxe:
~~~
    docker rmi <NomeDaImagem>
~~~

Exemplo:
~~~
    docker rmi hello-world
~~~

### **Flags principais:**
* `-i` -> Permite interagir com o container;
* `-t` -> Associa o seu terminal ao terminal do container ;
* `-it` -> É apenas uma forma reduzida de escrever -i -t;
* `--name` -> Permite atribuir um nome ao um container em execução;
* `-p n:m` -> Mapeia a porta **m** do container para a porta **n** do host;
* `-d` -> Executa o container em background;
* `-v /pasta/host:/pasta/container` -> Cria um volume '/pasta/container' dentro do container com o conteúdo da pasta '/pasta/host' do host.

### **Executar um comando no container:**

Sintaxe:
~~~
    docker exec <id_container ou nome_container> <path> <comando>
~~~

Exemplo:
~~~
    docker exec ab5386d99ed2 /bin/echo 'ola mundo' 
~~~

### **Entrar dentro de um container**

Sintaxe:
~~~
    docker attach <id_container ou nome_container> 
~~~

Exemplo:
~~~
    docker attach ab5386d99ed2 
~~~

### **Como subir novamente um container que já foi "morto":**

Sintaxe:
~~~
    docker start <id_container> 
~~~

Exemplo:
~~~
    docker start ab5386d99ed2 
~~~

### **Como "matar" um container em execução:**

Sintaxe:
~~~
    docker stop <id_container> 
~~~

Exemplo:
~~~
    docker stop ab5386d99ed2 
~~~

### **Como enviar uma imagem para o dockerhub:**

~~~
    docker push ...
~~~

## *`Criando suas proprias imagens:`*

### **DockerFile:**
O dockerfile nos permite contruir nossas próprias imagens.


### **Criando Dockerfile generico:**
* 1° Passo: Crie um arquivo Dockerfile, sem nenhuma extensão.

* 2° Passo: Agora abra esse arquivo no editor de texto de sua preferencia, para que possamos dar inicio aos comando.

* 3° Passo: Vamos definir uma imagem base para ser moficada:

Sintaxe:
~~~
    FROM <nomeDaImagem>
~~~

Exemplo:
~~~
    FROM nginx
~~~

* 4° Passo: Definir informações para a imagem (versão, descrição e autor):

Sintaxe:
~~~
    LABEL version=<versãoDesejada> description="Descricao da imagem" maintainer = <criadorDaImagem>
~~~

Exemplo:
~~~
    LABEL version="1.0.0" description = "Descricao da imagem generica" maintainer="Renan Kawamoto"
~~~

* 5° Passo: Executando comandos bash pelo script:

Sintaxe:
~~~
    RUN <comandoDoBash>
~~~

Exemplo:
~~~
    RUN cd / && mkdir Arquivos
~~~


* 6° Passo: Copiar um arquivo local para a Imagem:

Sintaxe:
~~~
    COPY <pathLocal> <pathDaImagem>
~~~

Exemplo:
~~~
    COPY ./teste/ola.txt /usr/teste2/
~~~

OBS: O " . " nesse caso se refere ao diretorio atual

* 7° Passo: Criando uma pasta "compartilhada" para todos os container que "herdarem" dessa imagem:

Sintaxe:
~~~
    VOLUME <pathDentroDoContainer>
~~~

Exemplo:
~~~
    VOLUME /arquivos/
~~~

*OBS:Caso você altere os elementos dentro desse diretorio os outros containeres que possuirem "herança" terão seus diretorios alterados igualmente, porém essa pasta não é refletida na maquina local.*

* 8° Passo: Definir portas que podem ser utilizadas na Imagem:

Sintaxe:
~~~
    EXPOSE <n°DaPorta>
~~~

Exemplo:
~~~
    EXPOSE 80
~~~

*OBS: Lembrando que a instrução EXPOSE não faz o mapeamento de porta, apenas deixa explícito que uma determinada porta pode ser mapeada durante a criação do container que utilizar essa imagem*

* 9° Passo: Setar algumas variaveis de ambiente:

Sintaxe:
~~~
    ENV <parametro>
~~~

Exemplo:
~~~
    ENV API_BANCO=meu_site
~~~

* 10° Passo: Definir qual pasta será iniciada quando acessarmos o container:

Sintaxe:
~~~
    WORKDIR <path>
~~~

Exemplo:
~~~
    WORKDIR /usr/teste
~~~

* 11° Passo: Definir um caminho para o comando cmd, e utilizar um comando cmd sempre que o container for iniciado:

Sintaxe:
~~~
    ENTRYPOINT [<path>]
    CMD [<comando>]
~~~

Exemplo:
~~~
    ENTRYPOINT ["/usr/sbin/nginx"]
    CMD ["-g", "daemon off;"]
~~~

### **Construir minha imagem com báse no dockerfile:**

Sintaxe:
~~~
    docker build -t <nomeDaImagem> <diretorio>
~~~

Exemplo:
~~~
    docker build -t minha-imagem:1.0 .
~~~
*OBS: Nesse caso a flag -t tem função de dar uma tag para a imagem*
## *`Visão geral dos comandos:`*

~~~
    attach      Anexar/Entrar a um contêiner em execução
    build       Construir uma imagem a partir de um Dockerfile
    commit      Crie uma nova imagem a partir das alterações de um contêiner
    cp Copy     arquivos / pastas entre um contêiner e o sistema de arquivos local
    create      Crie um novo contêiner
    diff        Inspecione as alterações no sistema de arquivos de um contêiner
    events      Obtenha eventos em tempo real do servidor
    exec        Execute um comando em um contêiner em execução
    export      Exportar o sistema de arquivos de um contêiner como um arquivo tar
    history     Mostra a história de uma imagem
    images      Listar imagens
    import      Importe o conteúdo de um tarball para criar um filesystem image
    info        Exibir informações de todo o sistema
    inspect     Retorne informações de baixo nível em um contêiner ou imagem
    kill        Mate um contêiner em execução
    load        Carregar uma imagem de um arquivo tar ou STDIN
    login       Faça login em um registro do Docker
    logout      Saia de um registro do Docker
    logs        Buscar os registros de um contêiner
    network     Gerenciar redes Docker
    pause       Pausar todos os processos em um contêiner
    port        Listar mapeamentos de portas ou um mapeamento específico para o CONTAINER
    ps          Listar containers
    pull        Extraia uma imagem ou repositório de um registro
    push        Envie uma imagem ou um repositório para um registro
    rename      Renomear um contêiner
    restart     Reinicie um contêiner
    rm          Remova um ou mais recipientes
    rmi         Remova uma ou mais imagens
    run         Execute um comando em um novo contêiner
    save        Salve uma ou mais imagens em um arquivo tar
    search      Pesquise imagens no Docker Hub
    start       Inicie um ou mais contêineres parados
    stats       Exibir uma transmissão ao vivo de estatísticas de uso de recursos de contêineres
    stop        Pare um contêiner em execução
    tag         Marque uma imagem em um repositório
    top         Exibir os processos em execução de um contêiner
    unpause     Retome todos os processos em um contêiner
    update      Atualizar configuração de um ou mais contêineres
    version     Mostra as informações da versão do Docker
    volume      Gerenciar volumes do Docker
    wait        Bloqueie até que um contêiner pare e, em seguida, imprima seu código de saída
~~~

*OBS:Todos esse comandos devem ser antecedidos por sudo docker*

