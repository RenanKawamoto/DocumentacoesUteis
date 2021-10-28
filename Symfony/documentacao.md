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

### **Parâmetros de rota:**

- Já definimos rotas estáticas(que não mudam) nessa documentação, porém tem cenários que rotas precisam ser dinamicas, sendo assim partes variáveis ​​são agrupadas entre `{}`e devem ter um nome único.

- Exemplo:
    ~~~yaml
        nome_da_rota:
            path:       /caminho_na_url/{parametro_da_rota_variavel}
            controller: App\Controller\NomeDaClasse::Método
    ~~~

    Nesse exemplo o parametro de rota `{parametro_da_rota_variavel}` é usado para criar uma variavel PHP onde o conteudo da rota é armazenado e passado para o controlador.

    `OBS: Para acessar você tem que colocar um atributo na sua função com o mesmo nome do parametro de rota.`

### **Validação de parâmetro de rota:**

- Imagine um cenario onde você tem duas rotas que aceitam parametros, sendo elas: `pagina/{numero}` e `pagina/{nome_da_pagina}`, o Symfony nos disponibiliza um recurso para diferencia-las.

- Esse recurso citado é o `requirement`(que tem que ser definida no arquivo de rotas):

    - Exemplo:
        ~~~yaml
            pagina_numero:
            path:       /pagina/{numero}
                controller: App\Controller\Pagina::lista
                requirements:
                    numero: '\d+'

            pagina_nome:
                path:       /pagina/{nome_da_pagina}
                controller: App\Controller\Pagina::nome
        ~~~

    - Nesse exemplo colocamos uma regra ao parametro de rota `{numero}`, definida por uma regra de expressão regular(no caso só aceita rotas com numeros) | Exemplo: /pagina/1213 -> cai na pagina_numero | Exemplo2: /pagina/ola_mundo -> cai na pagina_nome.

- Uma outra opção que é possível ser adotada é colocar o `requirement` entre `<>` dentro da propria rota:

    - Exemplo:
        ~~~yaml
            pagina_numero:
                path:       /pagina/{numero<\d+>}
                controller: App\Controller\Pagina::lista
        ~~~

### **Parâmetros opcionais:**

- Ao adicionar um parametro de rota, caso você tenha testado, você só conseguirá acessar a rota quando for passado um valor: Exemplo /pagina/1 -> você será redirecionado, mas caso coloque /pagina -> você não vai conseguir acessar.

- Para resolver esse problema o Symfony projetou uma solução, sendo ela o `defaults`, que é um parametro que você insere no seu arquivo de rotas.yaml, que tem  a unica e exclusiva função de passar um parametro como padrão caso o usuario não o faça:

- Exemplo:
    ~~~yaml
        pagina_numero:
                path:       /pagina/{numero}
                controller: App\Controller\Pagina::lista
                defaults:
                    numero: 1
    ~~~

    - Nesse exemplo caso o usuario digite `/pagina` na url, o Symfony passará como padrão `/pagina/1`

- Outra alternativa para esse métodos é:

    - Colocar um `?` após o parametro mostrando qual é o valor default:
        
        - Exemplo:
            ~~~yaml
                pagina_numero:
                    path:       /pagina/{numero?1}
                    controller: App\Controller\Pagina::lista
            ~~~

### **Conversão de parâmetro:**

- MAIS INFORMAÇÕES SOBRE ESSE TEMA QUE PRECISAM SER ADICIONADAS;

### **Parâmetros especiais:**

- Além de seus próprios parâmetros(definidos pelo programador), as rotas podem incluir qualquer um dos seguintes parâmetros especiais criados pelo Symfony:

    - `_controller`: Este parâmetro é usado para determinar qual controlador e ação são executados quando a rota é correspondida.

    - `_format`: O valor correspondido é usado para definir o "formato de solicitação" da Request. Isso é usado para coisas como definir o Content-Type da resposta (por exemplo, um json se traduz em um Content-Type application/json).

    - `_fragment`: Usado para definir o identificador de fragmento, que é a última parte opcional de um URL que começa com um **#** e é usado para identificar uma parte de um documento.

    - `_locale`: Usado para definir o local(região, tipo pt-br) na solicitação.

### **Parametros extras:**

- Caso você deseje passar mais parametros ao acessar uma rota, você pode ao envia-los pelo `defaults`:

- Exemplo:
    ~~~yaml
        index:
            path:       /index/{n}
            controller: App\Controller\Controller::método
            defaults:
                n: 1
                ola: "Ola mundo!"
    ~~~

### **Caracteres de barra em parâmetros de rota:**

- Como o caractere barra é especial ele não é aceito nos parametros de rotas, porém é possível fazer uma configuração para que a rota seja mais aceptivel, através do `requirements: parametro_de_rota: .+`:

- Exemplo:
    ~~~yaml
        compartilha:
            path:       /compartilha/{token}
            controller: App\Controller\Controller::compartilha
            requirements:
                token: .+
    ~~~

### **Grupos e prefixos de rotas:**

- MAIS INFORMAÇÕES SOBRE ESSE TEMA QUE PRECISAM SER ADICIONADAS;

### **Obtendo o nome e os parâmetros da rota:**

- O `Request` criado pelo Symfony armazena toda a configuração da rota (como o nome e parâmetros) nos "atributos de solicitação". 

- Exemplo:
    ~~~php
        use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
        use Symfony\Component\HttpFoundation\Request;
        use Symfony\Component\HttpFoundation\Response;

        class Controller extends AbstractController
        {
            public function list(Request $request): Response
            {
                $routeName = $request->attributes->get('_route');
                $routeParameters = $request->attributes->get('_route_params');
                $allAttributes = $request->attributes->all();

                ...
            }
        }
    ~~~

### **Redirecionando para URLs e rotas diretamente de uma rota:**

- Para redirecionar para uma rota utilizando o Symfony é muito simples, basta utilizar o controller `RedirectController` e passar como parametro `defaults` o `path`:

- Exemplo:
    ~~~
        Pagina:
            path: /pagina/{n}
            controller: Symfony\Bundle\FrameworkBundle\Controller\RedirectController
            defaults:
                path: '/oi/'
                n: 1
    ~~~

### **Redirecionando URLs com barras finais:**

- MAIS INFORMAÇÕES SOBRE ESSE TEMA QUE PRECISAM SER ADICIONADAS;

### **Roteamento de Subdomínio:**

- MAIS INFORMAÇÕES SOBRE ESSE TEMA QUE PRECISAM SER ADICIONADAS;

### **Rotas localizadas (i18n):**

- Se seu aplicativo for traduzido para vários idiomas, cada rota pode definir um URL diferente para cada local de tradução . Isso evita a necessidade de duplicar rotas, o que também reduz os possíveis bugs:

    - Exemplo:
        ~~~yaml
            about_us:
                path:
                    en: /about-us
                    pt-br: /sobre
                controller: App\Controller\CompanyController::about
        ~~~

- Um requisito comum para aplicativos internacionalizados é prefixar todas as rotas com uma localidade. Isso pode ser feito definindo um prefixo diferente para cada localidade (e configurando um prefixo vazio para sua localidade padrão, se preferir):

    - Exemplo: 
        ~~~yaml
            controllers:
            resource: '../../src/Controller/'
            type: annotation
            prefix:
                pt-br: '' 
                en: '/en'
        ~~~

- Outro requisito comum é hospedar o site em um domínio diferente de acordo com a localidade. Isso pode ser feito definindo um host diferente para cada local:

    - Exemplo:
        ~~~yaml
            controllers:
                resource: '../../src/Controller/'
                type: annotation
                host:
                    en: 'https://www.example.com'
                    pt-br: 'https://www.exemplo.com'
        ~~~

### **Rotas sem estado:**

- MAIS INFORMAÇÕES SOBRE ESSE TEMA QUE PRECISAM SER ADICIONADAS;

### **EXISTEM MAIS INFORMAÇÕES SOBRE OUTROS TÓPICOS, PORÉM NÃO OS CITAREI AQUI NESSE MOMENTO**

## - Controladores(Controllers):

### **Definindo um controller básico:**

- Embora um controlador possa ser qualquer PHP chamável (função, método em um objeto ou a Closure), um controlador geralmente é um método dentro de uma classe de controller.


### **A classe e os serviços do controlador de base:**

- Para ajudar no desenvolvimento, o Symfony vem com uma classe abstrata de controller base opcional chamada AbstractController. Ele pode ser estendido para obter acesso a métodos auxiliares.

- Exemplo:
    ~~~php
          namespace App\Controller;

            use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

            class NomeDaClasse_Controller extends AbstractController
            {
                // ...
            }
    ~~~

### **Redirecionamento:**

- Se você deseja redirecionar o usuário para outra página, use os métodos `redirectToRoute()` e `redirect()`:

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\RedirectResponse;
       
        //...

        public function index(): RedirectResponse
        {
            return $this->redirectToRoute('homepage');

            return $this->redirectToRoute('homepage', [], 301);

            return $this->redirectToRoute('app_lucky_number', ['max' => 10]);

            return $this->redirectToRoute('blog_show', $request->query->all());

            return $this->redirect('http://symfony.com/doc');
        }
    ~~~

    OBS: A diferença entre esses métodos é que o `redirect` não verifica o seu destino de forma alguma.

    OBS2: O redirecionamento tem que ser para o nome da rota.

### **Buscando serviços:**

- Serviços são classes e funcionalidades úteis que já vem embutidas no Symfony (Eles são usados ​​para renderizar modelos, enviar e-mails, consultar o banco de dados e qualquer outro "trabalho" que você possa imaginar).

-  Para vê-los, use o `debug:autowiring` no console:

    - Exemplo:
        ~~~
            php bin/console debug:autowiring
        ~~~

### **Gerando controladores:**

- Para economizar tempo, você pode instalar o `Symfony Maker` e dizer ao Symfony para gerar uma nova classe de controlador:

    - Exemplo:
        ~~~
            php bin/console make:controller BrandNewController
        ~~~

- Com o Symfony Maker é possível criar até mesmo cruds inteiros, porém não citarei aqui para não tomar muito tempo.

### **Gerenciando erros e páginas 404:**

- Quando as coisas não são encontradas, você deve retornar uma resposta 404. Para fazer isso, lance um tipo especial de exceção, utilizando o método `createNotFoundException` da classe `NotFoundHttpException`:

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\Response;
        use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;

        // ...
        public function index(): Response
        {
            $product = ...;
            if (!$product) {
                throw $this->createNotFoundException('Pagina não encontrada');
            }

            return $this->render(...);
        }
    ~~~

### **O objeto Request como um Argumento do Controlador:**

- E se você precisar ler parâmetros de consulta, obter um cabeçalho de solicitação ou obter acesso a um arquivo carregado? Essas informações são armazenadas no Request objeto do Symfony. Para acessá-lo em seu controlador, adicione-o como um argumento e digite-o com a classe Request:

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\Request;
        use Symfony\Component\HttpFoundation\Response;
        // ...

        public function index(Request $request, string $firstName, string $lastName): Response
        {
            $page = $request->query->get('page', 1);

            // ...
        }
    ~~~

### **Gerenciando a Sessão:**

- Symfony fornece um objeto de sessão que você pode usar para armazenar informações entre as solicitações. A sessão está habilitada por padrão, mas só será iniciada se você ler ou escrever a partir dela.

- O armazenamento da sessão e outras configurações podem ser controlados na configuração framework.session em config/packages/framework.yaml.

    - Para obter a sessão, adicione um argumento e tipe ele com `SessionInterface`:

        ~~~php
            use Symfony\Component\HttpFoundation\Response;
            use Symfony\Component\HttpFoundation\Session\SessionInterface;
            // ...

            public function index(SessionInterface $session): Response
            {
                $session->set('foo', 'bar');

                $foobar = $session->get('foobar');

                $filters = $session->get('filters', []);

                // ...
            }
        ~~~

### **Mensagens Flash:**

- Você também pode armazenar mensagens especiais, chamadas de mensagens "flash", na sessão do usuário. Por design, as mensagens flash devem ser usadas exatamente uma vez: elas desaparecem da sessão automaticamente assim que você as recupera. Esse recurso torna as mensagens "flash" particularmente excelentes para armazenar notificações do usuário.

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\Request;
        use Symfony\Component\HttpFoundation\Response;
        // ...

        public function update(Request $request): Response
        {
            // ...

            if ($form->isSubmitted() && $form->isValid()) {
                // do some sort of processing

                $this->addFlash(
                    'notice',
                    'Your changes were saved!'
                );
                // $this->addFlash() is equivalent to $request->getSession()->getFlashBag()->add()

                return $this->redirectToRoute(...);
            }

            return $this->render(...);
        }
    ~~~

### **Requests e Responses:**

- Tanto requests como responses que são classes do HttpFoundation possuem métodos para suas configurações e muitos outros, porém aqui não irei exemplifica-los e explica-los, para não tomar tanto tempo.

### **Retornando resposta JSON:**

- Para retornar JSON de um controlador, você pode usar o método `json`. Ele retornara um objeto JsonResponse que codifica os dados automaticamente:

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\Response;
        // ...

        public function index(): Response
        {
            return $this->json(['username' => 'jane.doe']);
        }
    ~~~

### **Respostas de arquivos de streaming:**

- Você pode usar o auxiliar `file()` para disponibilizar arquivos de dentro de um controlador:

- Exemplo:
    ~~~php
        use Symfony\Component\HttpFoundation\Response;
        // ...

        public function download(): Response
        {
            // send the file contents and force the browser to download it
            return $this->file('/path/to/some_file.pdf');
        }
    ~~~

## - Criando e usando templates:





