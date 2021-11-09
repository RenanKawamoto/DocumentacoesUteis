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
    - `attributes`: sem equivalente - usado pro seu aplicativo para armazenar outros dados;
    - `files`: equivalente a $_FILES;
    - `server`: equivalente a $_SERVER;
    - `headers`: principalmente equivalente a um subconjunto de $_SERVER ($request->headers->get('User-Agent'))

- Cada propriedade é uma instância de `ParameterBag`(ou uma subclasse de), que é uma classe de suporte de dados.

- Todas as instâncias de ParameterBag tem métodos para recuperar e atualizar seus dados:

    - `all()`: retorna todos os pametros;
    - `keys()`: retorna as chaves de parametro;
    - `replace()`: substitui os parametros atuais por um novo conjunto;
    - `add()`: adiciona parâmetros;
    - `get()`: retorna um parametro por nome;
    - `set()`: define um parametro por nome;
    - `has()`: retorna true se o parametro for definido;
    - `remover()`: remove um parametro;
