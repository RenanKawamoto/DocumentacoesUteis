# **Documentação básica sobre Docker Compose**

## *`Sumário:`*
1.
2.
3.

Essa documentação tem a função de explicar de maneira simples o que é e como funciona o Docker Compose.

## *`O que é?:`*
Docker Compose é uma ferramenta para definir e rodar multiplos containers em uma aplicação Docker.

Com ele é possível configurar serviços e aplicações, de maneira simples e intuitiva. Um exemplo dessa simplicidade é o fato de ser possível iniciar todos os serviços e suas configurações em ordem, sem muitos problemas.

## *`Como instalar:`*

### **No Linux:**
https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-20-04-quickstart-pt

### **No Windows:**
...

### **No Mac:**
...

## *`Estrutura de um arquivo docker-compose.yml:`*

Sintaxe:
~~~
    version: <VersãoQueQuerUtilizarDoComposer>
    services:
        <nomeDoServico1>:
            image: <NomeDaImagem>
            container_name: <NomeDoContainer>
            depends_on:
                - <NomeDoServico>
            volumes:
            <path>/<diretorioDoHost>:<path>/<diretorioDoContainer>
        <nomeDoServico2>:
            image: <Imagem>
            container_name: <NomeDoContainer2>
~~~

Exemplo:
~~~
    version: '3.4'
    services:
        db
            container_name: "Banco"
            build:
                context: ./banco
                dockerfile: Dockerfile
        aplicacao:
            depends_on:
                - db
            volumes:
                /home/usr/pastaCompartilhada:/compartilhadada
            container_name: "App"
            image: alpine
~~~

