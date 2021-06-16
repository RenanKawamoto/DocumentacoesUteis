# **Documentação básica do composer**

## *Sumário:*
1.
2.

## *Introdução:*

### **O que é o composer:**
- O composer é uma ferramenta de gerenciamento de dependencias para o PHP (muito semelhante ao pip do python, ou npm do node, caso já tenha utilizado).

- Ele irá instalar e atualizar as depencias que você definiu que seu programa necessita.

### **O composer é igual ao Apt ou Yum?**
- Não, o composer trata sim dos pacotes e bibliotecas para o usuario, porém ele os gerencia por projeto, instalando-os em um diretorio.

### **Como instalar o composer no linux:**
- O composer tem duas formas de ser instalado, sendo elas:

    - `Localmente:`
        - Ao instalar o composer localmente, você terá que baixar um arquivo e executa-lo no diretorio atual, toda vez que quiser gerenciar seus pacotes com o composer:

            - **Processo de download:**
                - Rode os seguintes comando pelo prompt no diretorio que quer utilizar:
                    ~~~
                        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

                        php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

                        php composer-setup.php

                        php -r "unlink('composer-setup.php');"
                    ~~~

        - **Executando o arquivo para o composer entrar em funcionamento**
            - Para isso devemos rodar o **composer.phar**, da seguinte forma:   
                ~~~php
                    php composer.phar install
                ~~~
            - **OBS:** PARA QUE O COMANDO FUNCIONE É NECESSÁRIO EXISTIR UM ARQUIVO NO DIRETORIO CHAMADO: "composer.json" QUE SERÁ EXPLICADO MAIS A FRENTE.

    - `Globalmente:`
        - Ao instalar o composer globalmente você poderá executa-lo em qualquer diretorio, utilizando o comando "composer" em vez de "php composer.phar":

            - **Processo de download:**
                - Rode os seguintes comando pelo prompt no diretorio que quer utilizar:
                    ~~~
                        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

                        php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

                        php composer-setup.php

                        php -r "unlink('composer-setup.php');"
                    ~~~

            - **Movendo o arquivo composer.phar para faze-lo funcionar globalmente:**

                - No diretorio onde se encontra o seu composer.phar, rode:
                    ~~~
                        mv composer.phar /usr/local/bin/composer
                    ~~~

            - **Executando o composer**
            - Para isso devemos rodar o **composer**, da seguinte forma:   
                ~~~php
                    composer install
                ~~~
            - **OBS:** PARA QUE O COMANDO FUNCIONE É NECESSÁRIO EXISTIR UM ARQUIVO NO DIRETORIO CHAMADO: "composer.json" QUE SERÁ EXPLICADO MAIS A FRENTE.

## *Uso básico do composer:*

### **Explicações:**

- Para que possamos entender melhor como funciona o básico do composer, usaremos o "monolog/monolog", que se trata de uma biblioteca de login.

### **Composer.json: o arquivo de configuração de projeto:**

- O composer.json se trata de um arquivo inicial para você utilizar o composer, uma vez que dentro dele é contido as dependencias utilizas e opcionalmente alguns métadados;

- OBS: Técnicamente você pode utilizar o composer em qualquer diretorio, porém caso você deseje publicar seu pacote no "Packagist.org" é necessário que ela esteja no topo do seu repositório VCS.

- `Comandos dentro do composer.json:`
    - **require:**
        - O require tem a função de especifica quias pacotes o seu projeto depente:

        - Sintaxe:
            ~~~
                {
                    "require":
                    {
                        "<nomeDoCriador>/<nomeDoPacote>" : "<versão>"
                    }
                }
            ~~~

        - Exemplo:
            ~~~
                {
                    "require":
                    {
                        "monolog/monolog": "2.0.*"
                    }
                }
            ~~~

        - O composer irá usar essa informação para encontrar o arquivo correto dentro dos repositorios de pacotes(que podem ser registrados com 'repositories') ou usando o repositorio padrão que é o Packagist.org;

        - *Package names*: os nomes de pacotes que são colocado no require, são constituido por nome do fornecedor do pacote / nome do pacote, porém esses comumente são iguais, visto que o fornecedor existe somente para não haver conflito de nomes;

        - *Package version constraints*: as restrições de versões dos pacotes são colocadas logo após os package names, e tem a função de selecionar uma versão especifica para nosso projeto, em nosso exemplo queremos uma versão 2.0.(qualquer numero);

### **Instalando as dependencias:**

- **OBS:** NOS EXEMPLOS A SEGUIR UTILIZAREI O COMPOSER INSTALADO DE MANEIRA GLOBAL, SENDO ASSIM CASO VOCÊ TENHA INSTALADO DE MANEIRA LOCAL BASTA SUBSTITUIR O COMANDO: 'composer ...' para 'php composer.phar ...';

- Existem duas maneiras de fazer isso:

    - Primeira forma(apenas instala):
        ~~~
            composer install
        ~~~
        - OBS: tem que ser feito na pasta que o arquivo composer.json está.

    - Segunda forma(instala e atualiza se for necessário):
        ~~~
            composer update
        ~~~

- Ao fazer a instalação das dependencias da segunda maneira ocorrem dois processos:
    - O composer baixa as depencias necessárias listads no seu arquivo composer.json e grava todos os pacotes e suas versões exatas no arquivo **composer.lock**, bloqueando o projeto para a versão escolhida.

    - É rodado implicitamente o comando "composer install", e esse irá fazer o download dos arquivos de dependencia e irá coloca-los dentro da pasta **vendor**(essa pasta é um diretorio convencional que contem os recursos de terceiros)

    - OBS: É interessante commitar o seu composer.lock caso esteja usando o git, uma vez que ele irá fazer que todos que peguem a mesma versão que você está utilizando no seu programa.

- *Usando o composer install, quando já existe um composer.lock:*

    - Ao utilizar o comando "composer install", quando já existe um composer.lock dentro do diretorio, faz com que o composer gerencie os pacotes para a versão presente no lock.

### **Atualizando as dependencias para a versão mais recente:**

- Para isso utilizamos o comando "composer update", que irá buscar qual a ultima versão presente em seu composer.json e atualizar o composer.lock.

- OBS: Caso você deseje instalar, atualizar ou remover uma dependencia, você pode passa-la como argumento:

    - Exemplo:
        ~~~
            composer update monolog/monolog 2.0.0
        ~~~

### **Packagist:**

- O packagist é um repositorio padrão (sendo assim, você não precisa especificar onde o composer deve buscar o pacote) e principal do composer;

- O packagist é um site, sendo assim é possível acessa-lo e buscar os pacotes a sua escolha.

### **Autoloading:**

- Para bibliotecas que especificam informações de carregamento automático(autoloading), o Composer gera um arquivo em: "vendor/autoload.php". Assim você pode incluir este arquivo e começar a usar as classes que essas bibliotecas fornecem sem nenhum trabalho extra.

- OBS: Você pode até adicionar seu próprio código ao autoloader adicionando um campo "autoload" ao composer.json.

## *Bibliotecas:*

- Nessa parte da documentação será explicado como tornar sua bliblioteca instalavel, através do composer.

- Para composer, assim que você possui um composer.json em seu diretorio, o seu diretorio se torna um pacote, a unica diferença entre um pacote real e seu diretorio é o nome.

### **Adicionando um nome ao seu pacote:**

- Para isso é necessário adicionar em seu composer.json um novo atributo, o "name".

- Sintaxe:
    ~~~json
        {
            "name": "<nomeDoFornecedor>/<nomeDoPacote>",
            "require":{
                "<nomeDoFornecedorDoPacote>/<nomeDoPacote>": "<versão>"
            }
        }
    ~~~

- Exemplo:
    ~~~json
        {
            "name": "teste/pacoteteste",
            "require":{
                "monolog/monolog": "1.0.*"
            }
        }
    ~~~

- OBS: O name tem que ser em lower case.


### **Versionando sua biblioteca:**

- O composer versiona seu projeto de duas maneiras, a primeira é se você estiver usando uma ferramente de versionamento como o git, nesse caso ele irá colocar a versão da sua lib seguindo a do git, caso você não esteja é necessário utilizar um comando ("version") para fazer isso manualmente no composer.json.

- Sintaxe:
    ~~~
        "version": "1.0.0"
    ~~~

### **Publicando seu pacote em um VCS:**

- Para isso basta postar seu diretorio que contenha o composer.json em um repositorio a sua escolha.

- **Como pegar o pacote de um repositorio VCS:**

    - Para isso basta em nosso composer.json adicionarmos uma clausula "repositories:[]":

    - Sintaxe:
        ~~~json
            "repositories":
            [
                {
                    "type": "vcs",
                    "url": <urlDoRepositorio>
                }
            ],
            "require":
            {
                "<fornecedorDoPacote/Pacote>": "<branchQueEstaOPacote>"
            }
        ~~~

    - Exemplo:
        ~~~json
            {
                "repositories": [
                    {
                        "type": "vcs",
                        "url": "https://github.com/username/hello-world"
                    }
                ],
                "require": {
                    "acme/hello-world": "dev-master"
                }
            }
        ~~~

### **Publicando no packagist:**

- Como vimos anteriormente é possível colocarmos nossos pacotes em repositorios para assim pordermos utiliza-los com composer e a clausula "repositories", porém isso não é muito pratico.

- Para resolver esse problema temos o packagist, que se trata de um repositorio padrão para os pacotes em php, assim ao registrar seu pacote no site não é necessário colocar a clausula "repositories".

- Para fazer isso basta entrar no site do Packagist e ir em submit.

## *Comandos básicos do composer pelo prompt:*

### **Opções globais:**

- Essas opções podem ser chamadas com todos os comandos do composer:

    - `--verbose(-v):` Adicionar mensagens que costumam ser ocultas.

    - `--help(-h):` Mostra as informações de ajuda referente ao comando.

    - `--quiet(-q):` Não mostra mensagens de saida.

    - `--no-interaction(-n):` Não faz nenhuma pergunta interativa.

    - `--no-plugin:` Desabilita os plugins

    - `--no-cache:` Desativa o uso do diretório de cache.

    - `--working-dir(-d):` Se fornecido, irá usar o diretorio que vier após ele como diretorio padrão para executar os comandos.

    - `--profile:` Define as informações de temo e uso da memoria.

    - `--ansi:` Força uma saida ANSI.

    - `--no-ansi:` Desabilita a saida ANSI.

    - `--version(-V):` Exibi a versão do composer.

### **Retorno do código:**
- **0**:OK;
- **1**:Erro desconhecido;
- **2**:Código de erro de resolução de dependência;

### **Comando para criar um composer.json de maneira interativa (init):**

~~~
    composer init
~~~

- **Opções para o comando init:**
    - `--name:` Passa o nome do fornecedor e o nome do pacote (fornecedor/pacote);
    - `--description:` Passa a descricao do pacote;
    - `--author:` Passa o nome do autor do pacote;
    - `--type:` Passa o tipo do pacote;
    - `--homepage:` Passa  a pagina inicial do pacote;
    - `--require:` Passa qual é o pacote que deseja instalar e sua versão da seguinte forma: "foo/bar:1.0.0".
    - `--stability (-s):` Valor para o campo "minimum-stability".
    - `--licence(-l):` Licensa do pacote.
    - `--repository:` Passa um ou mais repositorios, para a busca de pacotes.
    - `--autoload(-a):` Adicione um mapeamento de carregamento automático PSR-4 ao composer.json. Mapeia automaticamente o namespace do seu pacote para o diretório fornecido.

### **Comando install/i:**

- O comando install lê o arquivo composer.json do diretório atual, resolve as dependências e as instala.

- Se houver um arquivo **composer.lock** no diretório atual, ele usará as versões exatas de lá em vez de resolvê-las. Isso garante que todos os usuários da biblioteca obterão as mesmas versões das dependências.

- Se não houver um arquivo composer.lock, o Composer criará um após a resolução da dependência.

- **Opções para o comando install:**
    -...

### **Comando update/u:**

- Esse comando tem a função de obter as versões mais recentes das dependencias e atualizar o seu composer.lock.

- Caso não deseje atualizar todos os pacotes, você pode listar os pacotes que quer atualizar da seguinte forma:

    - Sintaxe:
        ~~~
            composer update vendor/packge vendor/packge2
        ~~~

- Se quiser fazer o downgrade de um pacote para uma versão específica sem alterar seu composer.json, você pode usar --with e fornecer uma restrição de versão personalizada:

    - Sintaxe:
        ~~~
            composer update --with vendor/package:2.0.1
        ~~~

- **Opções para o comando update:**
    -...

### **Comando require:**

- O comando `require` adiciona novos pacotes ao arquivo composer.json do diretório atual. Se nenhum arquivo existir, um será criado imediatamente.

- Você com esse comando também é possível passar vesões especificas caso desejar:

    - Sintaxe:
        ~~~
            composer require "monolog/monolog:2.*"
        ~~~

- **Opções para o comando require:**
    -...

### **Comando remove:**

- O comando `remove` tem a função de excluir um pacote do seu composer.json presente no seu diretorio atual.

- Sintaxe:
    ~~~
        composer remove monolog/monolog
    ~~~

- **Opções para o comando remove:**
    -...

### **Comando reinstall:**

- O comando de reinstalação procura pacotes instalados por nome, desinstala-os e reinstala-os. Isso permite que você faça uma instalação limpa de um pacote se você mexer com seus arquivos.

- **Opções para o comando reinstall:**
    -...

### **Comando check-platform-reqs:**

- Esse comando tem a função de verificar se suas versões de PHP e extensões correspondem aos requisitos de plataforma dos pacotes instalados.

### **Comando global:**

- O comando global permite que você execute outros comandos como install, remove, require ou update como se você os estivesse executando a partir do diretório COMPOSER_HOME.

### **Comando search:**

- Esse comando tem a função de fazer uma busca por pacotes no repositorio padrão (packagist).

- **Opções para o comando search:**
    -...

### **Comando show:**

- Tem a função de listar os pacotes instalados e suas versões.

- Caso você queira saber mais sobre algum pacote basta colocar seu nomeDoFornecedor/nomeDoPacote:

- Exemplo:
    ~~~
        composer show monolog/monolog
    ~~~

- **Opções para o comando show:**
    -...

### **Comando outdated:**

- Esse comando tem a função de mostrar uma lista de pacotes instalados que tem atualização disponivel.

- Código de cores:
    - verde (=): a dependência está na versão mais recente e atualizada;
    -amarelo (~): A dependência tem uma nova versão disponível que inclui quebras de compatibilidade com versões anteriores, então atualize quando puder, mas pode envolver trabalho adicional.
    - vermelho (!): A dependência tem uma nova versão que é sempre compatível e você deve atualizá-la.

- **Opções para o comando outdated:**
    -...


