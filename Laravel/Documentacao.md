# **Documentação básica do Laravel, segundo a doc oficial:**

## *Sumário:*
1.
2.
3.

## *Routing(Roteamento):*

### **- Basic Routing(roteamento básico):**

- O Laravel oferece uma forma muito simples de criar uma rota, sem exigir qualquer arquivo de configuração.

- Para isso basta usar a Classe estatica Route, com seu verbo, a URI e o seu fechamento:

- Sintaxe:
    ~~~php
        Route::<verbo>("<URI>", <fechamento>);
    ~~~

- Exemplo:
    ~~~php
        Route::get("/hello", function()
        {
            return "Hello World";
        })
    ~~~

##### **The Default Route Files(O arquivo padrão de rotas):**

- Todas as rotas são definidas em arquivos de rota, que se encontram no diretorio "routes".

- Todos os arquivos de rotas são carregados automaticamente pelo "App\Providers\RouteServiceProvider".

- O arquivo "routes\web.php" define quais são as rotas da sua interface web. Sendo assim as rotas contidas nesse arquivo são atribuidas ao grupo de middleware, que fornece recursos como estado de sessão e proteção contra CSRF.

- Já as rotas presentes em "routes/api.php" não possuem estado e são atribuidas ao grupo de middleware "api".

> Para a maioria das aplicações você irá definir suas rotas dentro de "routes/web.php", pois as rotas que forem definidas nesse arquivo podem ser acessadas por meio da URL em seu navegador.

- As rotas definidas dentro de "routes/api.php" são aninhadas dentro de um grupo de rota pelo RouteServiceProvider. Dentro desse grupo, o prefixo "/api" URI é aplicado automaticamente para que você não precise aplicá-lo manualmente a todas as rotas no arquivo.

OBS: Você pode modificar o prefixo e outras opções de grupo de rota, modificando sua classe RouteServiceProvider.

- Exemplo de rota no arquivo api.php:
    ~~~php
        Route::get("/ola", 
        function(){
            return "ola";
        });
    ~~~

    - Agora para acessar essa rota basta enviar uma requisição get para "/api/ola".

#### **Available Router Methods(Métodos de roteamento disponiveis):**

- A classe Route permite registrar rotas com qualquer verbo HTTP.

~~~php
    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);
~~~

- As vezes precisamos registrar uma rota que responde a diversos verbos HTTP. Nesses cenários você pode usar o método `match`. Ou fazer com que a rota responda a todos o verbos com o metodo `any`:

    - Sintaxe:
        ~~~php
            Route::match(['get', 'post'], '/', function)(){
                //
            })

            Route::any('/', function(){
                //
            });
        ~~~

#### **CSRF Protection:**

- Ao utilizar o laravel é necessário que os formularios HTML (post, put, patch e delete) contenham o token "@csrf", mais a frente será explicado melhor como funciona o CSRF:

- Sintaxe:
    ~~~php
        <form methos="POST" action="/">
            @csrf
            ...
        </form>
    ~~~

#### **Redirect Routes(Redirecionar rotas):**

- Se você definir uma rota, que redireciona para outra URI, você pode usar o metódo "redirect". Esse método facilita o processo de redirecionamento simples:

- Sintaxe:
    ~~~php
        Route::redirect("<rotaAtual>", "<rotaQueIrá>");
    ~~~

- Exemplo:
    ~~~php
        Route::redirect("/ola", "/");
    ~~~

- Por padrão o redirect retorna um status 302, porém você pode alterar esse, com um terceiro parametro que é opcional:

- Sintaxe:
    ~~~php
        Route::redirect("<rotaAtual>", "<rotaQueIrá>", 307);
    ~~~

#### **View Routes:**

- Se você tem uma rota que tem a função de apenas retornar uma view, é possível fazer esse processo de maneira simplificado, apenas utilizando o método "view":

- Sintaxe:
    ~~~php
        Route::view("<URI>", "<nomeDaView>");
    ~~~

- Exemplo:
    ~~~php
        Route::view("/welcome", "welcome");
    ~~~

- OBS: Caso desejar você pode passar um terceiro argumento, sendo ele um array de parametros que serão passados para a view:

    - Exemplo:
        ~~~php
            Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
        ~~~

### **- Route Parameters(Parametros de Rotas):**

#### **Required Parameters(Parametros requeridos):**

- As vezes precisamos obter um dado pela URI, sedo assim podemos utilizar o required parameters, da seguinte maneira:

- Sintaxe:
    ~~~php
        Route::get("/user/{<nomeDoParametro>}", function()
        {
            ...
        });
    ~~~

- Exemplo:
    ~~~php
        Route::get("/user/{nome}", function($nome)
        {
            return "ola ".$nome;
        });
    ~~~

- Você pode definir quantos parametros de rotas necessitar:

    - Exemplo:
        ~~~php
            Route::get('/user/{name}/age/{age}', function($name, $age)
            {
                return "ola ".$name." voce tem ".$age." anos";
            });
        ~~~

- Os parametros de rotas tem que estar entre "{ }" e ser uma sequencia de caracteres alphanumericos, também é aceito " _ "  

#### **Optinal Parameters(Paremetros de rotas opcional):**

- As vezes você precisa de um parametro, que é opcional para o funcionamento do sistema, sendo assim podemos fazer isso nas rotas, utilizando o "optinal parameter" colocando um " ? " após o parametro:

- Sintaxe:
    ~~~php
        Route::get("/user/{<nomeDoParametro>?}", function(...)
        {
            ...
        })
    ~~~

- Exemplo:
    ~~~php
        Route::get("/user/{name?}", function($name = null)
        {
            return $name;
        })
    ~~~

- OBS: Não esqueça de atribuir um valor padrão caso use o parametro opcional.

#### **Regular Expression Constraints(Restrições com expressões regulares):**

- Com o Laravel é possível você restringir o formato dos seus parametros de rota usando o método where na sua instancia:

- Sintaxe:
    ~~~php
        Route::get('/user/{<parametro>}', function(...)
        {
            ...
        })-> where('<parametro>', '<Expressão regular');
    ~~~

- Exemplo:
    ~~~php
        Route::get('/user/{id}', function ($id) {
            //
        })->where('id', '[0-9]+');

        Route::get('/user/{id}/{name}', function ($id, $name) {
            //
        })->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
    ~~~

- Por conveniencia o laravel já possui alguns padrões de expressões regulares implementado:

- Exemplo:
    ~~~php
        Route::get('/user/{id}/{name}', function ($id, $name) {
            //
        })->whereNumber('id')->whereAlpha('name');

        Route::get('/user/{name}', function ($name) {
            //
        })->whereAlphaNumeric('name');

        Route::get('/user/{id}', function ($id) {
            //
        })->whereUuid('id');
    ~~~

- OBS: Se o parametro passado não corresponder ao padrão esperado será retornado um 404;

#### **Global Constraints(Restrições globais):**

- Caso você deseje fazer uma restrição de da rota de maneira global, basta utilizar o método "pattern" dentro do App\Provider\RouteServiceProvider:

- Exemplo:
    ~~~php
        public function boot()
        {
            Route::pattern('id', '[0-9]+');
        }
    ~~~

- Uma vez que o padrão foi definido, ele é automaticamente aplicado a todas as rotas usando aquele nome de parâmetro.

#### **Named Routes(Rotas nomeadas):**

- As rotas nomeadas tem a função de facilitar a organização das nossas rotas, facilitando por exemplo os redirects. Você pode especificar um nome para uma rota através do método "name()" que é concatenado com o método HTTP do Laravel:

- Sintaxe:
    ~~~php
        Route::get('/', function(){
            //
        })->name('<nomeDaRota>');
    ~~~

- Exemplo:
    ~~~php
        Route::get("/user/{name?}", function($name = null)
        {
            return $name;
        })->name('nameUser');
    ~~~

#### **Generating URLs To Named Routes(Gerando URLs para rotas nomeadas):**

- Após atribuir um nome a uma rota, você pode gerar URLs ou redirecionar os caminhos para elas através dos métodos auxiliares: "route" e "redirect":

- Sintaxe:
    ~~~php
        // Generating URLs...
        $url = route('<nomeDaRota>');

        / Generating Redirects...
        return redirect()->route('<nomeDaRota>');

    ~~~

- Exemplo:
    ~~~php
        // gerando a rota
        Route::get('/url', function(){
            $url = route('nomeDaRota');
            return $url // irá retornar ip:porta/<URI>
        });

        Route::get('/redirect', function()
        {
            return redirect()->route('NomeDaRota');
        })
    ~~~

- Se a rota nomeada contem parametros é possível passa-los através de um segundo argumento do método route:

    - Exemplo:
        ~~~php
            Route::get('/user/{id}/profile', function ($id) {
                //
            })->name('profile');

            $url = route('profile', ['id' => 1]);
        ~~~

- Se você passar parâmetros adicionais na matriz, esses pares de chave / valor serão adicionados automaticamente à string de consulta do URL gerado.

### *Route Groups(Grupos de rotas):*

- Os grupos de rota permitem que você compartilhe atributos de rota, como middleware, em um grande número de rotas sem a necessidade de definir esses atributos em cada rota individualmente.

- Os grupos aninhados tentam "mesclar" de forma inteligente os atributos com seu grupo.

    - Middleware e "where" , são condições mescladas enquanto os nomes e prefixos são acrescentados.

    - Delimitadores de namespace e barras nos prefixos da URI são adicionados automaticamente quando apropriado.

#### **Middleware:**

- Para atribuir um middleware para todas rotas de um grupo, você pode usar seu metodo antes de definir o grupo:

- Exemplo:
    ~~~php
        Route::middleware(['first', 'second'])->group(function () {
            Route::get('/', function () {
                // Uses first & second middleware...
            });

            Route::get('/user/profile', function () {
                // Uses first & second middleware...
            });
        });
    ~~~

- OBS: Os middlewares são executados em ordem que foram listados no array.

#### **Subdomain Routing(Roteamento de subdominio):**

- Os grupos de rota também podem ser usados ​​para lidar com o roteamento de subdomínio. Subdomínios podem ser atribuídos a parâmetros de rota, assim como URIs de rota, permitindo que você capture uma parte do subdomínio para uso em sua rota ou controller.

- O subdomian pode ser especificado ao chamar o método "domain" antes da definição do grupo:

- Exemplo:
    ~~~php
        Route::domain('{account}.example.com')->group(function () {
            Route::get('user/{id}', function ($account, $id) {
                //
            });
        });
    ~~~

#### **Route Prefixes(Prefixo de rota):**

- O método "prefix" pode ser usado para prefixar cada rota no grupo com um determinado URI:

- Exemplo:
    ~~~php
        Route::prefix('admin')->group(function () {
            Route::get('/users', function () {
                // Matches The "/admin/users" URL
            });
        });
    ~~~

#### **Route Name Prefixes(Prefixo de nome da rota):**

- O método "name" também é uma forma de prefixar as rotas no grupo, porém com a diferença que o prefixo será uma string, sendo assim tudo que tiver nele será literalmente interpretado:

- Exemplo:
    ~~~php
        Route::name('admin.')->group(function () {
            Route::get('/users', function () {
                // Route assigned name "admin.users"...
            })->name('users');
        });
    ~~~

### **Fallback Routes(Rotas alternativas):**

- O método "Route::fallback", tem como atribuição ser executado, quando nenhuma das rotas conseguirem atender a requisição, portanto por padrão irá retornar um 404, porém você pode alter isso utilizando-o:

- Exemplo:
    ~~~php
        Route::fallback(function () {
            return "error";
        });
    ~~~

- OBS: o fallback, sempre deve ser a última rota registrada na sua aplicação.

### **Rate Limiting(Limite de taxas):**

#### **Defining Rate Limiters(Definindo limitadores de taxas):**

- O rate limit é uma maneira simples que o Laravel nos da de configurar restrições de trafico em rotas ou grupo de rotas.

- Normalmente, essa configuração deve ser feita no método "configureRateLimiting" da classe "App \ Providers \ RouteServiceProvider".

...

### **Form Method Spoofing(Falsificação de método de formulário):**

- Formulários HTML não suportam ações PUT, PATCH ou DELETE. Portanto, ao definir rotas PUT, PATCH ou DELETE que são chamadas de um formulário HTML, você precisará adicionar um campo "_method" oculto ao formulário. O valor enviado com o campo _method será usado como o método de solicitação HTTP

- Exemplo:
    ~~~php
        <form action="/example" method="POST">
            <input type="hidden" name="_method" value="PUT">
            <input type="hidden" name="_token" value="{{ csrf_token() }}">
        </form>
    ~~~

- OBS: Por conveniencia, você pode usar o @method do blade para gerar um "_method" input:

    - Exemplo:
        ~~~php
            <form action="/example" method="POST">
                @method('PUT')
                @csrf
            </form>
        ~~~

### **Accessing The Current Route(Acessando a rota atual):**

- Você pode usar os métodos "current", "currentRouteName" e "currentRouteAction" da "Route" para acessar informações sobre a rota:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\Route;

        $route = Route::current(); // Illuminate\Routing\Route
        $name = Route::currentRouteName(); // string
        $action = Route::currentRouteAction(); // string
    ~~~

### **Route Caching(Cache de rotas):**

- Ao implantar sua aplicação para produção, você deve aproveitar as vantagens do cache de rotas do Laravel. Usar o cache de rota diminuirá drasticamente a quantidade de tempo que leva para registrar todas as rotas do seu aplicativo.

- Para gerar um cache de rota, execute o artisan route:cache:

- Exemplo:
    ~~~php
        php artisan route:cache;
    ~~~

- Lembre-se, se você adicionar novas rotas, você precisará gerar um novo cache de rota. Por isso, você só deve executar o comando route:cache durante a implantação do seu projeto.

- Você pode usar o comando route: clear para limpar o cache da rota:

- Exemplo:
    ~~~php
        php artisan route:clear;
    ~~~

## *Middleware:*

- Os middlewares são mecanismos conveniente para inspecionar e filtrar solicitações HTTP que entram em sua aplicação. Por exemplo, o Laravel inclui um middleware que verifica se o usuário do seu aplicativo está autenticado.

- Existem vários middlewares incluídos no framework Laravel, incluindo middleware para autenticação e proteção CSRF. Todos esses middleware estão localizados no diretório "app / Http / Middleware"

### **Defining Middleware(Definindo Middleware):**

- Para criar um novo middleware, basta utilizar o comando make do artisan:

- Exemplo:
    ~~~php
        php artisan make:middleware NomeDoMiddleware
    ~~~

- OBS: Ao criar um middleware com o artisan será gerado um código padrão e nesse tera o comando "return $next($request);", que possui como função a deixar que a requisição passe normalmente.

#### **Middleware & Responses(Middleware e Respostas):**

- Os middlewares podem realizar tarefas antes ou depois de passar a request.

- Exemplo antes da request:
~~~php
    <?php

    namespace App\Http\Middleware;

    use Closure;

    class BeforeMiddleware
    {
        public function handle($request, Closure $next)
        {
            // Perform action

            return $next($request);
        }
    }
~~~

- Exemplo depois da request:
~~~php
    <?php

    namespace App\Http\Middleware;

    use Closure;

    class AfterMiddleware
    {
        public function handle($request, Closure $next)
        {
            $response = $next($request);

            // Perform action

            return $response;
        }
    }
~~~

### **Registering Middleware(Registrando Middleware):**

#### **Global Middleware(Middleware Global):**

- Se você deseja que um middleware seja executado durante cada solicitação HTTP para sua aplicação, liste a classe de middleware na propriedade $ middleware de sua classe "app / Http / Kernel.php."

#### **Assigning Middleware To Routes(Atribuindo Middleware a Rotas):**

- Se desejar atribuir middleware a rotas específicas, você deve primeiro atribuir ao middleware uma chave(nome) no arquivo "app / Http / Kernel.php" dentro da propriedade "$routeMiddleware":

- Exemplo:
    ~~~php
        protected $routeMiddleware = [
            'auth' => \App\Http\Middleware\Authenticate::class,
            'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
            'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
            'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
            'can' => \Illuminate\Auth\Middleware\Authorize::class,
            'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
            'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
            'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
            'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
        ];
    ~~~

- Uma vez que você já tenha nomeado seu middleware, basta utilizar o método "middleware(<nomeDoMiddleware>)" na sua rota:

- Exemplo:
    ~~~php
        Route::get('/profile', function () {
            //
        })->middleware('auth');
    ~~~

- Você pode atribuir vários middlewares à rota, passando uma matriz de nomes de middleware para o método:

- Exemplo:
    ~~~php
        Route::get('/', function () {
            //
        })->middleware(['first', 'second']);
    ~~~

- Outra opção é você passar para o método middleware, o nome completo da classe:

- Exemplo:
    ~~~php
        use App\Http\Middleware\EnsureTokenIsValid;

        Route::get('/profile', function () {
            //
        })->middleware(EnsureTokenIsValid::class);
    ~~~

- OBS: ao fazer dessa maneira não há necessidade de alterar o kernel.

#### **Middleware Groups(Grupos de Middleware):**

- Às vezes, você pode querer agrupar vários middleware em uma única chave para torná-los mais fáceis de atribuir a rotas. Você pode fazer isso usando a propriedade "$middlewareGroups" em seu kernel HTTP.

- Exemplo:
    ~~~php
        protected $middlewareGroups = [
            'web' => [
                \App\Http\Middleware\EncryptCookies::class,
                \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
                \Illuminate\Session\Middleware\StartSession::class,
                // \Illuminate\Session\Middleware\AuthenticateSession::class,
                \Illuminate\View\Middleware\ShareErrorsFromSession::class,
                \App\Http\Middleware\VerifyCsrfToken::class,
                \Illuminate\Routing\Middleware\SubstituteBindings::class,
            ],

            'api' => [
                'throttle:api',
                \Illuminate\Routing\Middleware\SubstituteBindings::class,
            ],
        ];
    ~~~

- Os grupos de middleware tornam mais conveniente atribuir muitos middlewares a uma rota de uma vez:

- Exemplo:
    ~~~php
        Route::get('/', function () {
        //
        })->middleware('web');

        Route::middleware(['web'])->group(function () {
            //
        });
    ~~~

- OBS: Por padrão, os grupos de middleware "web" e "api" são aplicados automaticamente aos arquivos "routes/web.php" e "routes/api.php" correspondentes do seu aplicativo pelo "App\ Providers\RouteServiceProvider".

#### **Sorting Middleware(Classificando Middleware):**

- Caso você deseje alterar a ordem de prioridade dos seus middlewares, basta alterar o "$middlewarePriority" que fica em "app/Http/Kernel.php".

- OBS: Essa propriedade não vem por padrão.

### **Middleware Parameters(Parâmetros de Middleware):**

- Você pode passar parametros adicionais para o middlewares, após o $next:

- Exemplo:
    ~~~php
        <?php

        namespace App\Http\Middleware;

        use Closure;

        class EnsureUserHasRole
        {
            /**
            * Handle the incoming request.
            *
            * @param  \Illuminate\Http\Request  $request
            * @param  \Closure  $next
            * @param  string  $role
            * @return mixed
            */
            public function handle($request, Closure $next, $role)
            {
                if (! $request->user()->hasRole($role)) {
                    // Redirect...
                }

                return $next($request);
            }
        }
    ~~~

- Os parâmetros de middleware podem ser especificados ao definir a rota, separando o nome e os parâmetros do middleware com um ":" :

- Exemplo:
    ~~~php
        Route::put('/post/{id}',  function ($id) {
        //
        })->middleware('role:editor');
    ~~~


