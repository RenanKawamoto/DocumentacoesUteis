# **Documentação básica sobre o Guzzle:**

## *Sumário:*
1.
2.
3.

## *O que é o Guzzle?*

- Guzzle é um cliente PHP HTTP que torna fácil enviar solicitações HTTP e trivial para integrar com serviços da web.

## *Instalação:*

- Para instalar o Guzzle, utilizaremos o composer, sendo assim basta rodar o comando abaixo:

~~~
    composer require guzzlehttp/guzzle:^7.0
~~~

## *Entendendo o básico:*

###  **- Fazendo uma requisição:**

- Você pode enviar requisições com o Guzzle, usando o objeto GuzzleHttp\ClientInterface;

- OBS: não esqueça de usar o "require 'vendor/autoload.php'", no inicio do seu código;

- #### **Criando um Cliente:**
    - Para criar um cliente com o Guzzle, basta usar a classe `Client`, presente em `GuzzleHttp\Client`, essa classe possui um construtor que aceita um array associativo, com diversos parametros opcionais, exemplo:
        - `base_uri`: É a URL base que será usada para unir com as URIs.

    - Exemplo de Client:
        ~~~php
            require 'vendor/autoload.php';
            use GuzzleHttp\Client;

            $client = new Client(
                [
                    'base_uri' => 'http://httpbin.org',
                    'timeout' => 2.0,
                ]
            )
        ~~~

- #### **Enviando requisições:**
    - O métodos mágicos presente na classe cliente facilitam muito o envio de requisições:

    - Exemplo:
        ~~~php
            $response = $client->get('http://httpbin.org/get');
            $response = $client->delete('http://httpbin.org/delete');
            $response = $client->head('http://httpbin.org/get');
            $response = $client->options('http://httpbin.org/get');
            $response = $client->patch('http://httpbin.org/patch');
            $response = $client->post('http://httpbin.org/post');
            $response = $client->put('http://httpbin.org/put');
        ~~~

    - OBS: caso você já tenha definido o Cliente com base_uri, você pode substituir o "http://httpbin.org/get" apenas por 'get', visto que 'http://httpbin.org', é a URL padrão.

    - Outra maneira de fazer esse processo é criando a requisição e depois envia-la pelo cliente:

    - Exemplo:
        ~~~php
            use GuzzleHttp\Psr7\Request;

            $request = new Request('PUT', 'http://httpbin.org/put');
            $response = $client->send($request, ['timeout' => 2]);
        ~~~
        
        - OBS: não esquecer do use;

- #### **Requisições assincronas:**
    - Você pode fazer essas requisições usando os métodos mágicos presente em client:

    - Exemplo:
        ~~~php
            $promise = $client->getAsync('http://httpbin.org/get');
            $promise = $client->deleteAsync('http://httpbin.org/delete');
            $promise = $client->headAsync('http://httpbin.org/get');
            $promise = $client->optionsAsync('http://httpbin.org/get');
            $promise = $client->patchAsync('http://httpbin.org/patch');
            $promise = $client->postAsync('http://httpbin.org/post');
            $promise = $client->putAsync('http://httpbin.org/put');
        ~~~

    - Ou você pode usar os métodos `sendAsync()` e `requestAsync()` de client:

    - Exemplo:
        ~~~php
            use GuzzleHttp\Psr7\Request;

            // Create a PSR-7 request object to send
            $headers = ['X-Foo' => 'Bar'];
            $body = 'Hello!';
            $request = new Request('HEAD', 'http://httpbin.org/head', $headers, $body);
            $promise = $client->sendAsync($request);

            // Or, if you don't need to pass in a request instance:
            $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
        ~~~

    - Após fazer a requisição async você pode ver seu resultado através do método `then()`. Essas chamadas são atendidas com um "Psr \ Http \ Message \ ResponseInterface" quando bem-sucedido e geram uma exceção se forem rejeitadas:

    - Exemplo:
        ~~~php
            use Psr\Http\Message\ResponseInterface;
            use GuzzleHttp\Exception\RequestException;

            $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
            $promise->then(
                function (ResponseInterface $res) {
                    echo $res->getStatusCode() . "\n";
                },
                function (RequestException $e) {
                    echo $e->getMessage() . "\n";
                    echo $e->getRequest()->getMethod();
                }
            );
        ~~~

- #### **Requisições concorrente:**
    - Você pode mandar multiplas requisições usando as promises e as asynchronous requests;

    ...

### **- Usando as responses:**
    
- As responses são objetos que implementam uma response PSR-7, Psr\Http\Message\ResponseInterface, que contém muitas informações uteis:

- Você pode usar o método `getStatusCode()` para pegar o n° do status, e pode usar o método `getReasonPhrase()` para pegar a frase que foi retornada na response;

- Exemplo:
    ~~~php
        $code = $response->getStatusCode(); // 200
        $reason = $response->getReasonPhrase(); // OK
    ~~~

- Você pode recuperar o header da response, com o método `getHeader()` e pode saber se existe um header utilizandoo `hasHeader()`:

- Exemplo:
    ~~~php
        // Check if a header exists.
        if ($response->hasHeader('Content-Length')) {
            echo "It exists";
        }

        // Get a header from the response.
        echo $response->getHeader('Content-Length')[0];

        // Get all of the response headers.
        foreach ($response->getHeaders() as $name => $values) {
            echo $name . ': ' . implode(', ', $values) . "\r\n";
        }  
    ~~~

- Você pode recuperar o body usando o método `getBody`, além disso você pode usar o método `read(n°DeLinhas)` para pegar um numero exato de linhas do body, e o método `getContents()` que pega todo o conteudo do body:

- Exemplo:
    ~~~php
        $body = $response->getBody();
        // Implicitly cast the body to a string and echo it
        echo $body;
        // Explicitly cast the body to a string
        $stringBody = (string) $body;
        // Read 10 bytes from the body
        $tenBytes = $body->read(10);
        // Read the remaining contents of the body as a string
        $remainingBytes = $body->getContents();
    ~~~

###  **- Parametros de consulta(na url):**

- Você pode passar esses parametros de três maneiras:

    - 1° - Diretamente na URL da requisição:
        - Exemplo:
            ~~~php
                $response = $client->request('GET', 'http://httpbin.org?foo=bar');
            ~~~
    - 2° - Usando um array associativo com o parametro `query`:
        - Exemplo:
            ~~~php
                $client->request('GET', 'http://httpbin.org', [
                    'query' => ['foo' => 'bar']
                ]);
            ~~~

    - 3° - Usando um array associativo com uma string:
        - Exemplo:
            ~~~php
                $client->request('GET', 'http://httpbin.org', ['query' => 'foo=bar']);
            ~~~

### **- Carregando dados:**

- Existem diversas maneiras de fazer isso com guzzle, segue alguns exemplo:
~~~php
    use GuzzleHttp\Psr7;

    // Provide the body as a string.
    $r = $client->request('POST', 'http://httpbin.org/post', [
        'body' => 'raw data'
    ]);

    // Provide an fopen resource.
    $body = Psr7\Utils::tryFopen('/path/to/file', 'r');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);

    // Use the Utils::streamFor method to create a PSR-7 stream.
    $body = Psr7\Utils::streamFor('hello!');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);
~~~

- Uma maneira simples de fazer upload de um Json é usando o header apropriado e mandando a opção json na request:

- Exemplo:
    ~~~php
        $r = $client->request('PUT', 'http://httpbin.org/put', [
            'json' => ['foo' => 'bar']
        ]);
    ~~~

### **- POST/Form requests:**

- Além de especificar os dados brutos de uma solicitação usando a opção 'body', o Guzzle fornece abstrações úteis sobre o envio de dados POST.

- #### **Mandando campos do formulario:**

- O envio de solicitações POST em "application / x-www-form-urlencoded" requer que você especifique os campos POST como uma matriz nas opções de solicitação form_params:

- Exemplo:
    ~~~php
        $response = $client->request('POST', 'http://httpbin.org/post', [
            'form_params' => [
                'field_name' => 'abc',
                'other_field' => '123',
                'nested_field' => [
                    'nested' => 'hello'
                ]
            ]
        ]);
    ~~~

- #### **Mandando arquivos para um formulario:**

- Para você mandar arquivos, é necessário utilizar o `multipart request`. Multipart é uma opção que aceita um array associativo de arrays, em que cada associação deve conter:
    - name: nome do campo do formulario;
    - contets: o conteudo do campo;

- Exemplo:
    ~~~php
        use GuzzleHttp\Psr7;

        $response = $client->request('POST', 'http://httpbin.org/post', [
            'multipart' => [
                [
                    'name'     => 'field_name',
                    'contents' => 'abc'
                ],
                [
                    'name'     => 'file_name',
                    'contents' => Psr7\Utils::tryFopen('/path/to/file', 'r')
                ],
                [
                    'name'     => 'other_file',
                    'contents' => 'hello',
                    'filename' => 'filename.txt',
                    'headers'  => [
                        'X-Foo' => 'this is an extra header to include'
                    ]
                ]
            ]
        ]);
    ~~~

### **- Cookies:**



