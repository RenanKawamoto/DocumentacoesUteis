# **Criando imagens para Docker**

Essa documentação, tem a funcionalidade de explicar como criar uma imagem para docker de maneira simplificada e prática.

Para iniciarmos esse processo, é necessário entender as imagens que criaremos são providas de um arquivo `Dockerfile`, que não possui extensão. Sendo assim, caso deseje fazer esse tutorial, é necessário criar o arquivo.

*Dentro do Docker file:*

Sintaxe:
~~~
    FROM <imagem-que-a-sua-ira-se-basear>
    LABEL description=<"decrição"> version=<"numero-da-versão"> maintainer=<"criador-da-imagem">
    RUN <codigo-shell>
    COPY <path/arquivo-que-quer-copiar-para-o-container path/diretorio-que-quer-que-o-arquivo-copiado-fique>
    VOLUME <path/diretorio>
    EXPOSE <n°-da-porta>
    ENV <parametro>
    WORKDIR <path/dir>
    ENTRYPOINT [<path>]
    CMD [<comando>]
~~~

* `FROM` -> Após essa instrução é colocado qual imagem que a sua irá usar como base para ser construida; 
* `LABEL` -> Após essa instrução é colocado uma descrição, versão e criador;
* `RUN` -> Após essa instrução é colocado um comando do shell, que será rodado (Exemplo: echo 'ola mundo');
* `COPY` -> Após essa instrução é colocado ao lado esquerdo "onde em sua maquina fica o arquivo que você deseja copiar para o container que herdar dessa imagem" e ao lado direto "onde o arquivo será colocado no container";
* `VOLUME` -> Após essa instrução é colocado um diretorio que ficará dentro do diretorio padrão no seu host e no diretorio definido no container;
* `EXPOSE` -> Compartilha uma porta do seu container pra ser acessada pela sua maquina;
* `ENV` -> Após essa instrução virá um comando para setar uma variavel de ambiente;
* `WORKDIR` -> Após essa instrução virá um diretorio que será aberto sempre que o usuario tentar entrar no container;
* `ENTRYPOINT` -> Após essa instrução vem o caminho onde o comando cmd será executado;
* `CMD` -> Após essa instrução vem um comando cmd, que será executado sempre o container foi iniciado.


Exemplo:
~~~
    FROM alpine
    LABEL description="alpine para desenvolvimento" version="1.0.0" maintainer="Pessoa Sobrenome"
    RUN mkdir pasta
    COPY ./pasta/texto.txt /home/pasta/ 
    VOLUME /home/compartilhado/
    EXPOSE 80
    ENV API_BANCO=meu_site
    WORKDIR /home/usr
    ENTRYPOINT ["/usr/sbin/nginx"]
    CMD ["-g", "daemon off;"]
~~~

### **Como buildar sua imagem:**

Sintaxe:
~~~
    docker build -t <nomeDaImagem> <diretorio>
~~~

Exemplo:
~~~
    docker build -t minha-imagem:1.0 .
~~~