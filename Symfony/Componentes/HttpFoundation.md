# **Componente: HttpFoundation:**

## *Sumário*:
1.
2.
3.

## *Entendendo o HttpFoundation:*

### **Função básica:**

- O componente HttpFoundation define uma camada orientada a objetos para as expecificações HTTP;

- Em PHP, o pedido é representado por algumas variáveis globais ( $_GET, $_POST, $_FILES, $_COOKIE, $_SESSION, ...) e a resposta é gerada por algumas funções ( echo, header(), setcookie(), ...).

- Já com o HttpFoundation essas variáveis ​​globais são substituidas por uma camada orientada a objetos.

### **Instalação:**

- Para instalar esse pacote é necessário ter o `composer` intalado em sua máquina, após isso basta utilizar o seguinte comando:

    ~~~composer
    composer require symfony/http-foundation
    ~~~

## *Request(solicitação):*

- A maneira mais comum de criar uma solicitação é baseado-a nas já conhecidas super globais PHP, porém em nosso caso usaremos o método estático `createFromGlobals()` da classe Request:

    ~~~php
    use Symfony\Componente\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    ~~~

## **Acessando dados de solicitações:**

- Um objeto Request contém informações sobre a solicitação do cliente. Essas informações podem ser acessadas por meio de propriedades públicas:
    - `request`: equivalente a $_POST;
    - `query`: equivalente a $_GET($request->query->get('name'));
    - `cookies`: equivale a $_COOKIE;
    - `attributes`: sem equivalente - usado no seu aplicativo para armazenar outros dados;
    - `files`: equivalente a $_FILES;
    - `server`: equivalente a $_SERVER;
    - `headers`: principalmente equivalente a um subconjunto de $_SERVER ($request->headers->get('User-Agent'))

- Cada propriedade é uma instância de `ParameterBag`(ou uma subclasse de), que é uma classe de suporte de dados.

- Todas as instâncias de ParameterBag tem métodos para recuperar e atualizar seus dados:

    - `all()`: retorna todos os pametros:
        
        ~~~php
        <?php
        
        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::creteFromGlobals();
        var_dump($request->query->all()); 
        ~~~

    - `keys()`: retorna as chaves de parametro:
        
        ~~~php
        <?php
        
        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        var_dump($request->query->keys());
        ~~~

    - `replace()`: substitui os parametros atuais por um novo conjunto:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->replace(['chave' => 'valor']);
        var_dump($request->query->all());
        ~~~

    - `add()`: adiciona parâmetros;

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->add(['chave' => 'valor']);
        var_dump($request->query->all());
        ~~~

    - `get()`: retorna um parametro por nome;

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        var_dump($request->query->get('nomeDoParametroSolicitadoNaQueryString', 'valorPadraoCasoEleNaoExista'));
        ~~~

    - `set()`: define um parametro por nome:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', 'valor');
        echo $request->query->get('chave');
        ~~~

    - `has()`: retorna true se o parametro for definido:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', 'valor');
        var_dump($request->query->has('chave'));
        ~~~

    - `remove()`: remove um parametro:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', 'valor');
        $request->query->remove('chave');
        var_dump($request->query->all());
        ~~~

- Outra caracteristica das instâncias de ParameterBag é a presença de métodos para filtrar os seus resultados:

    - `getAlpha()`: retorna caracteres alfabeticos:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave','valor12');
        var_dump($request->query->getAlpha('chave'));
        ~~~

    - `getAlnum()`: retorna caracteres alfabeticos e numericos:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', 'valor22');
        var_dump($request->query->getAlnum('chave'));
        ~~~

    - `getBoolean()`: retorna o parametro convertendo seu valor para um booleano:

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', '1');
        var_dump($request->query->getBoolean('chave'));
        ~~~

    - `getDigits()`: retorna os digitos do parametro solicitado(esse virá como sendo uma string):

        ~~~php
        <?php

        require 'vendor\autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', 'valor78');
        var_dump($request->query->getDigits('chave'));
        ~~~

    - `getInt()`: retorna o valor do parametro solicitado porém convertido para inteiro:

        ~~~php
        <?php
        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('chave', '189');
        var_dump($request->query->getInt('chave'));
        ~~~

    - `filter()`: o método filter usa a função `filter_var()` do php para filtrar o parametro:

        ~~~php
        <?php
        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        $request->query->set('email', 'email@gmail.com');
        var_dump($request->query->filter('email', null/* esse parametro valor padrão caso não encontre o parametro passado*/, FILTER_VALIDATE_EMAIL));
        ~~~

- `Uma caracteristica importante do método get() é que ele não da suporte a parametros de tipo array, sendo assim é necessário usar outro método, no caso o all():`
    
    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $request->query->set('chave', ['chaveDoArray' => 'valor do array']);
    var_dump($request->query->all()['chave']['chaveDoArray']);
    ~~~

- Um ponto muito interessante a ser mencionado é a propriedade publica `attributes`(que existe nas instâncias de ParameterBag), onde você pode guardar informações que pertencem à solicitação e que precisam ser acessadas de muitos pontos diferentes em seu aplicativo.

- Caso você seja observador deve ter percebido, que até agora não foi mostrado nenhuma maneira para acessar o body das requisições, e por esse motivo passaremos para esse tópico:

    - `Como acessar os dados "brutos" vindos de um body de uma request (basta utilizar o método getContent()):`

        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        var_dump($request->getContent()); 
        ~~~

    - `Como acessar os dados em formato de array vindo de um body de uma request (json):`
        ~~~php
        <?php

        require 'vendor/autoload.php';
        use Symfony\Component\HttpFoundation\Request;

        $request = Request::createFromGlobals();
        var_dump($request->toArray());
        ~~~

## **Indentificando uma requisição (request):**

- As vezes em seu aplicativo você precisa indentificar uma solicitação e na maioria das vezes isso é feito por meio das "URI" da solicitação, que pode ser acessada por meio do método `getPathInfo()`:

    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    var_dump($request->getPathInfo());
    ~~~

## **Simulando uma requisição:**

- Em vez de criar uma requisição baseada nas superglobais do php, é possível com o HttpFoundation simular uma requisição:

    ~~~php
    $request = Request::create(
        '/uri',
        'GET',
        ['chave'=>'valor']
    )
    ~~~

- O método create recebe uma URI, um método(GET, POST, PUT ...) e alguns outros parametros(isso depende do método escolhido).

- Caso você deseje também é possível sobreescrever as superglobais post com o método: `overrideGlobals()`;

## **Acessando uma sessão atrelada a uma Request:**

- Se você tem um sessão atrelada a uma requisição você pode acessar ela via `getSession()` método presente na classe Request e RequestStack.

- Caso você queria conferir se uma requisição possui uma sessão que foi inicializada anterirormente, é possível utilizar o método `hasPreviousSession() `.

## **Processando headers http:**

- **...**

## **Acessando Accept-\*Dados de Cabeçalhos:**

- **...**

## **Anonimizando endereços IP:**

- **..**

## **Acessando outros dados:**

- A classe Request possui muitos outros métodos que você pode usar para acessar as informações da solicitação. Dê uma olhada na API Request para obter mais informações sobre eles (https://github.com/symfony/symfony/blob/5.3/src/Symfony/Component/HttpFoundation/Request.php).

## **Sobrescrevendo a request:**

- **...**

## **Response:**

- Um objeto da classe Reponse tem todas as informações que precisam ser enviadas para um cliente em uma determinada solicitação. O seu construtor aceita três argumentos: o conteúdo da response, o código de status e uma matriz de cabeçalhos HTTP:

    ~~~php
    <?php
    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Response;

    $response = new Response(
        '<html><h1>Ola mundo!</h1></html>',
        Response::HTTP_OK,
        ['content-type' => 'text/html']
    );
    ~~~

    OBS: Nesse momento a response foi criada, porém ainda não foi enviada para o cliente.

- Essas informações também podem ser manipuladas após a criação do objeto Response:

    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Response;
    $response = new Response();
    $response->setContent('<html><h1>Ola mundo</h1></html>');
    $response->headers->set('Content-Type', 'text/html');
    $response->setStatusCode(Response::HTTP_OK);
    ~~~


## **Enviando a response:**

- Antes de enviar a response, você pode utilizar o método `prapare()` para corrigir qualquer incompatibilidade com a especificação HTTP.

- Após esse processo você pode utilizar o método `send()` para enviar a response:

    ~~~php
    <?php
    require 'vendor/autoload.php';

    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $response = new Response(
        '<html><h1>Ola mundo</h1></html>',
        Response::HTTP_OK,
        ['content-type' => 'text/html']
    );

    $response->prepare($request);
    $response->send();
    ~~~

## **Configurado Cookies:**

- Os cookies das responses podem ser manipulados por meio do atributo público `headers`:

    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Cookie;
    use Symfony\Component\HttpFoundation\Response;

    $response = new Response(
        'conteudo',
        Response::HTTP_OK,
        ['content-type' => 'text/html']
    );

    $response->headers->setCookie(Cookie::create('chave', 'valor'));
    ~~~

    Como você pode ter percebido no atributo header temos o método `setCookie()`, que irá gerar para nos o nosso cookie (através de uma instancia da classe `Cookie`).

    OBS: a classe Cookie aprensenta diversos métodos, como getValue(), getName() e etc (esse podem ser muito uteis caso esteja tentando acessar, os valores presentes dentro dos cookies).

- Além do setCookie, podemos também "limpar o nosso cookie" com o método `cleanCookie()` (presente nas instancias de HeaderBag).

- Caso você queira criar ou alterar um cookie, é possível fazer isso através do métodos `->with*()`:

    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Cookie;
    use Symfony\Component\HttpFoundation\Response;

    $response = new Response(
        'conteudo',
        Response::HTTP_OK,
        ['content-type' => 'text/html']
    );
    $cookie = Cookie::create('nomeCookie', 'valorCookie')->withValue('novoValorDoCookie')->withExpires(strtotime('Fri, 20-May-2080 15:25:52 GMT'));
    $response->headers->setCookie($cookie);
    
    var_dump($response->headers->getCookies()[0]->getValue());
    ~~~

## **Gerenciando o Cache HTTP:**

- **...**

## **Redirecionando o cliente:**

- Para redirecionar os clientes, o HttpFoundation utiliza a classe `RedirectResponse()`:

    ~~~php
    <?php
    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\RedirectResponse;

    $redirect = new RedirectResponse('/olamundo');
    $redirect->send();
    ~~~

## **Transmitindo uma response:**

- **...**

## **Disponibilizando arquivos:**

- Ao enviar um arquivo para o usuario fazer download, você deve adicionar um header `Content-Disposition` a sua response. Embora a criação desse cabeçalho para downloads básicos de arquivos seja direta, o uso de nomes de arquivos não ASCII é mais complexo. Por esse motivo a classe HeaderUtils possui o método `makeDisposition()`:

    ~~~php
    <?php
    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\HttpFoundation\HeaderUtils;

    $conteudoQueApareceraNoArquivo = "Ola mundo";
    $response = new Response($conteudoQueApareceraNoArquivo);

    $disposition = HeaderUtils::makeDisposition(
        HeaderUtils::DISPOSITION_ATTACHMENT,
        'nomeDoArquivo.txt'
    );

    $response->headers->set('Content-Disposition', $disposition);
    $response->send();
    ~~~

- Caso você esteja disponibilizando(para a leitura, dentro do seu site) um arquivo estático, você pode usar a classe `BinaryFileResponse()`:

    ~~~php
    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\BinaryFileResponse;

    $caminho = 'texto.txt';
    $response = new BinaryFileResponse($caminho);
    $response->send();
    ~~~

    Você ainda assim pode usar o `HeaderUtils::makeDisposition()`, para disponibilizar o arquivo para download:

    ~~~php

    <?php

    require 'vendor/autoload.php';
    use Symfony\Component\HttpFoundation\BinaryFileResponse;
    use Symfony\Component\HttpFoundation\ResponseHeaderBag;
    $caminho = 'texto.txt';
    $response = new BinaryFileResponse($caminho);
    $response->headers->set('Conten-type', 'text/plain');
    $response->setContentDisposition(
        ResponseHeaderBag::DISPOSITION_ATTACHMENT,
        'nomeDoArquivo.txt'
    );
    $response->send();
    ~~~

- **...**

## **Crianção de uma response JSON:**









