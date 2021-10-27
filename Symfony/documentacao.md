# **Documentação básica do framework Symfony**

## *Sumário:*
1.
2.
3.

## - Instalação e configuração:

### **Requisitos tecnicos:**
- Na data atual(27/10/2021) é necessário ter uma versão igual ou superior ao PHP 7.2.5 e ter algumas extensões: *Ctype*, *Iconv*, *JSON*, *PCRE*, *Session*, *SimpleXML* e *Tokenizar* (essas costumam ser instaladas por padrão junto com o PHP).

- Outro requisito muito importante é possuir instalado o *composer*.

- De maneira opcional você pode instalar o `Symfony CLI`, que se trata de um binario para chamar o comando `symfony` pelo seu prompt e esse disponibiliza todas as ferramentas necessárias para trabalhar com o Symfony localmente.

    - **OBS:** Caso você tenha instalado o `Symfony CLI` você pode rodar um comando para saber se todos o requisitos aqui solicitados então certos:
        ~~~
            symfony check:requirements
        ~~~

### **Criando um projeto com Symfony:**

- Existem duas maneiras de criar um projeto com Symfony:
    
    - *Através do Symfony CLI:*
        
        ~~~
            symfony new nome_do_seu_projeto --full

            symfony new nome_do_seu_projeto
        ~~~

        - Utilizando o `symfony new nome_do_seu_projeto --full` você irá instalar todos os pacotes para você construir uma aplicação web.

        - Utilizando o `symfony new nome_do_seu_projeto` você irá instalar os pacotes necessários para criar um microserviço (uma aplicação console ou API).

    - *Através do composer:*

        ~~~
            composer create-project symfony/website-skeleton nome_do_seu_projeto


            composer create-project symfony/skeleton nome_do_seu_projeto
        ~~~

        - Utilizando o ` composer create-project symfony/website-skeleton nome_do_seu_projeto` você irá instalar todos os pacotes para você construir uma aplicação web.

        - Utilizando o `composer create-project symfony/skeleton nome_do_seu_projeto` você irá instalar os pacotes necessários para criar um microserviço (uma aplicação console ou API).

### **Rodando um projeto Symfony localmente:**

- Existem duas maneiras de executar um projeto Symfony localmente:
    
    - *Através do Symfony CLI:*
        ~~~
            symfony server:start
        ~~~

        OBS: Caso queira parar o servidor basta pressionar "ctrl + c" no seu terminal.
    
    - *Através do servidor local do PHP:*
        ~~~
            php -S locahost:8000
        ~~~

        OBS: Não esqueça de entrar na pasta public para rodar o comando acima.

### **Instalando pacotes:**

- Uma prática comum entre os desenvolvedores que utilizam o Symfony é a instalação(ou remoção) de novos pacotes (esses são chamados de bundles), dessa maneira cada pacote exige certas alterações para serem instalados, porém o Symfony desenvolveu uma maneira de automatizar esse processo, no caso esse recurso se chama `Symfony Flex`.

- O Symfony Flex se trata de uma extensão para o composer que altera o funcionamento de alguns comando do mesmo, quando se trata de projetos Symfony, automatizando alguns processos.

    - OBS: O Symfony Flex possui repositorios da comunidade, muito semelhante ao Packagist, sendo eles:

        - https://github.com/symfony/recipes (Main recipes repository) - É um repositório de pacotes de alta qualidades que são mantidos. Esse é o repositorio padrão que o Symfony flex irá buscar.

        - https://github.com/symfony/recipes-contrib (Contrib recipes repository) - É um repositorio de pacotes criados pela comunidade, todos funcionam, porém podem não possuir mais manutenção. O Symfony flex vai pedir sua permissão para instalar esses.

### **Conjunto de pacotes:**

- O Symfony disponibiliza uma serie de recursos conhecidos como packs, que se tratam de varios bundles juntos, para você não ter que intala-los manualmente um por um.

### **Verificiando vunerabilidades de segurança:**

- Caso você esteja utilizando o Symfony CLI, você rodar um comando que irá mostrar se seu código está com vunerabilidades:

    ~~~
        symfony check:security
    ~~~

## - Criando sua primeira página com o Symfony:

### **Rota e controlador:**

- Para iniciarmos o processo para gerar nossa primeira página vamos levar em consideração um cenário: nesse você deseja criar uma página que gere um numero aleatório toda vez que você a atualiza;

- Sendo assim primeiramente devemos criar um `Controller`(que é arquivo onde ficará a nossa lógica, retornando um resultado);

- Esse controller, como todos os outros, por uma questão de organização estrutural deve ser mandido dentro do diretorio: `src/Controller`:

    ~~~php
        <?php
        // src/Controller/NumeroDaSorte.php

        //caminho até esse arquivo
        namespace App\Controller;

        //importação da classe Response
        use Symfony\Component\HttpFoundation\Response;

        class NumeroDaSorte
        {
            //função publica que espera um retorno do tipo Response
            public function numero(): Response
            {
                //lógica para gerar um numero aleatorio de 0 a 100
                $n = random_int(0, 100);

                //retorno de uma instancia da classe Response
                return new Response(
                    '<html><body>O seu numero da sorte é: '.$n.'</body></html>'
                );
            }
        }
    ~~~

- Agora que já criamos o controller devemos vincula-lo a uma rota para que o usuario final possa acessar essa página com uma URL (Exemplo: www.nomeDoSeuSite/numero_da_sorte);

- Para fazermos isso é necessário alterar o arquivo presente em: `config/routes.yaml`:

    - Sintaxe:
        ~~~
            nome_da_rota:
                path: /rota
                controller: App\Controller\NomeDaClasse::nomeDoMetodo
        ~~~

    - Exemplo:
        ~~~
            NumeroDaSorte:
                path: /numero_da_sorte
                controller: App\Controller\NumeroDaSorte::numero
        ~~~

        OBS: como colocamos o path como /numero_da_sorte, para acessarmos a nossa lógica devemos colocar da seguinte forma: localhost:8000/numero_da_sorte.

### **O comando bin/console:**

- O Symfony disponibiliza uma ferramenta muito poderosa de debug, que é o `bin/console`;

- Para ver quais são os comandos e suas funções basta utilizar:
    ~~~
        php bin/console
    ~~~

- Para exemplificar as funções do bin/console e como ele pode ser util, basta utilizar o comando:

    ~~~
        php bin/console debug:router
    ~~~

    OBS: Esse comando irá mostrar todas as rotas disponiveis.

### **Renderizando um template:**

- Caso você queira dividir ainda mais sua aplicação, colocando todas as "views" (conteudo, como html, css e js), em outra pasta e retornando essas no seu controller é possível através de um bundle chamado `Twig` (muito semelhante ao blade, caso você já conheça o Laravel).

- Para iniciarmos a utilização do Twing, precisamos instala-lo no nosso projeto:

    ~~~
        composer require twig
    ~~~

    OBS: Você consegue fazer esse require devido a influencia do Symfony Flex presente no seu composer.

- Agora que fizemos isso precisamos alterar algumas coisas no nosso controller, para que assim ele possa entender o Twig:

    ~~~php
        <?php

            namespace App\Controller;

            use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
           
            use Symfony\Component\HttpFoundation\Response;

            class NumeroDaSorte extends AbstractController
            {
                ...
            }
    ~~~

    - Existem duas principais mudanças no nosso arquivo, a primeira é a presença do novo use `Symfony\Bundle\FrameworkBundle\Controller\AbstractController` (Adiciona a classe AbstractController) e a segunda é a presença do `extends AbstractController`, assim a nossa classe irá herdar todas as propriedades e métodos da classe AbstractController.

- Após esse processo vamos criar nosso primeiro template/view utilizando o Twig, por normas de estrutura todos os templates devem ficar em `/templates/pastaParaOTemplate/nomeDoTemplate.html.twig`:

    ~~~
        {# templates/NumeroDaSorte/numero.html.twig - isso é um comentario#}
        <h1>O seu numero da sorte é {{ n }}</h1>
    ~~~

    OBS: o código `{{n}}` é a sintaxe que o twig utiliza para pegar valores de variaveis passados pelo controller(como veremos a seguir).

- Como finalizamos a criação da nossa tela com o twig, basta editarmos o nosso controller para retornar esse template quando o usuario encontrar a rota que definimos:

    ~~~php
        namespace App\Controller;

        use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
        use Symfony\Component\HttpFoundation\Response;

        class NumeroDaSorte extends AbstractController
        {
            public function numero(): Response
            {
                $n = random_int(0, 100);

                return $this->render('NumeroDaSorte/numero.html.twig', [
                    'n' => $n,
                ]);
            }
        }
    ~~~

    `$this->render()` é um método que é herdado da classe AbstractController que tem a função de retornar uma Response com o conteúdo do template twig;

    OBS: Como você pode ver o controller manda para o template o valor `n`;

- Pronto com isso você já possui uma página simple que divide os templates do seus controllers 😁.

## - Encaminhamento/Roteamento:

- Existem diversas formas de definir rotas com o Symfony, sendo elas: por anotações, atributos, yaml, xml e php, porém por motivos didaticos, irei utilizar e exemplificar aqui apenas com o yaml. Caso queira mais informações sobre os outros métodos recomendo que vá na documentação oficial na seção de routing(https://symfony.com/doc/current/routing.html).

### **Criação de rotas pelos arquivos yaml:**

- A principal vantagem dessa forma é não requerem nenhuma dependência extra. A principal desvantagem é que você precisa trabalhar com vários arquivos ao verificar o roteamento de alguma ação do controlador.

- É importante salientar que os arquivos de rotas yaml só serão interpretados em dois diretorio: `/config/routes.yaml` e `/config/rotes/qualquerArquivo.yaml`.

- Exemplo:
    ~~~yaml
        nomeDaRota:
            path: /caminho_que_vai_ficar_na_url
            controller: App\Controller\NomeDaClasse::NomeDoMétodo
    ~~~

### **Como reestringir os verbos HTTP para as rotas:**

- Por padrão o Symfony cria as rotas aceitando todos os verbos HTTP(GET, POST, PUT, DELETE e etc), porém com a sintaxe `methods` você consegue reestringir as rotas:

    ~~~
        nomeDaRota:
            path: /caminho_que_vai_ficar_na_url
            controller: App\Controller\NomeDaClasse::NomeDoMétodo
            methods: GET 
    ~~~

    OBS: Caso você queira você pode colocar mais verbos utilizando o `|`, exemplo: `Exemplo: GET | POST`.

### **Expressões correspondentes:**

- FUTURAMENTE SERÁ ADICIONADO NA DOCUMENTAÇÃO

### **Rotas de depuração:**

- Conforme sua aplicação cresce vão surgindo mais rotas, o Symfony pensando nisso nos disponibiliza alguns recursos para ajudar na manutenção dessas:
    
    - O primeiro é o `bin/console debug:router` que irá imprimir todas as rotas da aplicação.

        - OBS: Caso você passe o nome ou parte do nome de uma rota para essa função será retornado os detalhes sobre a mesma, Exemplo: `bin/console debug:router nomeDaRota`.

    - O segundo é o `bin/console router:match` que irá mostrar qual é a rota da url fornecida:

        - Exemplo: `php bin/console router:match /numeroDaSorte`

### **Parametros de rota:**

