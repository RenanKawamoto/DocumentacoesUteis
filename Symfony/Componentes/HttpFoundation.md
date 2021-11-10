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

