# **O componente Routing do Symfony:**

## *Sumário:*
1.
2.
3.
4.

## *Instalação:*

- Para instalar esse pacote é necessário ter conhecimento sobre o gerenciador de pacotes "composer", caso já tenha esse basta rodar o seguinte comando:

    ~~~
    composer require symfony/routing
    ~~~

## *Preludio:*

- Caso deseje ter melhor base antes de entrar nesse módulo, é necessário estudar HttpFoundation (componente do symfony).

## *Configuração do sistema de rotas:*

- Um sistema de rotas usando o routing é dividido em três partes:

    - Uma `RouteCollection`, que é uma classe que conterá uma lista das suas rotas (essas são instancias da classe `Route`);

    - Uma `RequestContext`, que é uma classe com as informações sobre a request;

    - Uma `UrlMatcher`, que é uma classe que irá confirmar se certa rota existe dentro da sua `RouteCollection`;

## *Criando uma rota:*

- Para criar uma rota utilizaremos a classe `Route`:

    ~~~php
    <?php
    //require 'vendor/autoload.php';
    use Symfony\Component\Routing\Route;

    $route = new Route(
        '/caminho/{variavel}', // URI
        ['_controller' => 'showArchive'], // valores padrões que serão enviados
        ['month' => '[0-9]{4}-[0-9]{2}', 'subdomain' => 'www|m'], // requisitos
        [], // opções
        '{subdomain}.example.com', // host
        ['https'], // esquemas
        ['GET'], // métodos
        'context.getHost() matches "/(secure|admin).example.com/"' // condicionais
    );
    ~~~

## *RouteCollection:*

- A classe RouteCollection, como já foi citado anteriormente tem a função de armazenar as instancias Route, assim temos um controle de quais rotas existem.

- `Como adicionar rotas nas RouteCollections:`

    ~~~php
    <?php
    //require 'vendor/autoload.php';
    use Symfony\Component\Routing\Route;
    use Symfony\Component\Routing\RouteCollection;

    $route = new Route('/uri');
    $routes = new RouteCollection();
    $routes->add('nomeDaRota', $route);
    ~~~

- `Como adicionar caracteristicas para todas as rotas de uma RouteCollection:`

    - A classe RouteCollection, possui alguns métodos para você definir opções comuns dentro da mesma, afim de melhor e facilitar a codificação:

        ~~~php
        <?php
        //require 'vendor/autoload.php';

        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $rota = new Route('/teste');

        $rotas = new RouteCollection();
        $rotas->add('teste', $rota);

        $rotas->addPrefix('/prefix');
        $rotas->addDefaults(['']);
        $rotas->addRequirements(['']);
        $rotas->addOptions(['']);
        $rotas->setHost('{subdomain}.example.com');
        $rotas->setMethods(['POST']);
        $rotas->setSchemes(['https']);
        $rotas->setCondition('context.getHost() matches "/(secure|admin).example.com/"');
        ~~~

- `Como adicionar uma RouteCollection dentro de outra:`

    - A fim de melhorar a organização do seu código você pode dividir as rotas e depois junta-las em uma RouteCollection root, para isso essa classe disponibiliza o método `addCollection()`:

        ~~~php
        <?php
        //require 'vendor/autoload.php';

        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $rota = new Route('/a');
        $rotas_a_z = new RouteCollection();
        $rotas_a_z->add('a', $rota);

        $rota2 = new Route('/0');
        $rotas_0_9 = new RouteCollection();
        $rotas_0_9->add('0', $rota2);

        $rotas_principal = new RouteCollection();
        $rotas_principal->addCollection($rotas_a_z);
        $rotas_principal->addCollection($rotas_0_9);
        ~~~

## *Entendendo um pouco melhor a classe RequestContext:*

- Essa classe é bem simples, porém de inicio pode causar uma confusão. Ela tem uma unica função em nosso sitema, que é dar informações da requisição(request), muito semelhante a classe Request do componente HttpFoundation.

- Dentro dela existem vários métodos para recuperar dados da requisição, citarei alguns aqui:

    - `getBaseUrl()`: recupera Url base;

    - `getMethod()`: recupera o método HTTP da requisição;

    - `getHost()`: recupera o host da requisição;

    - `getScheme()`: recupera se é http ou https;

    - `getHttpPort()`: recupera a porta da requisição http;

    - `getHttpsPort()`: recupera a porta da requisição https;

    - `getPathInfo()`: recupera o URI;

    - `getQueryString()`: recupera a query string;

- Caso você não esteja usando o component HttpFoundation, ao criar uma instancia de RequestContext() você deve usar a superglobal $_SERVER para adicionar os valores solicitados no construtor;

- Caso você esteja usando o component HttpFoundation, basta utilizar o método `fromRequest()` para que o RequestContext "puxe" os dados da requisição:

    ~~~php
    <?php
    //require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\RequestContext;

    $context = new RequestContext();
    $context->fromRequest(Request::createFromGlobals());

    var_dump($context->getPathInfo());
    ~~~


















