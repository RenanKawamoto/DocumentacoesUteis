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


## *CSRF Protection:*

- As falsificações de solicitação entre sites são um tipo de exploração mal-intencionada em que comandos não autorizados são executados em nome de um usuário autenticado. Felizmente, o Laravel facilita a proteção do seu aplicativo contra ataques de falsificação de solicitação entre sites (CSRF).

#### **An Explanation Of The Vulnerability(Uma explicação da vunerabilidade):**

- Caso você não esteja familiarizado com essa falha de segurança, irei explicar com um exemplo, imagine que você tenham uma rota que necessita que o usuario esteja autenticado para que assim ele possa alterar seu email, com CSRF o atacante iria criar um form HTML que aponta para sua rota já estando autenticado, assim poderia mudar seu email sem dificuldades (Se o site mal-intencionado enviar automaticamente o formulário quando a página for carregada, o usuário mal-intencionado só precisa atrair um usuário desavisado do seu aplicativo para visitar o site dele e o endereço de e-mail dele será alterado no seu aplicativo.). 

- Para evitar essa vulnerabilidade, precisamos inspecionar cada solicitação POST, PUT, PATCH ou DELETE de entrada para um valor de sessão secreta que o aplicativo malicioso não consegue acessar.

### **Preventing CSRF Requests(Prevenção de solicitações de CSRF):**

- O Laravel gera automaticamente um "token" CSRF para cada sessão de usuário ativa gerenciada pela aplicação. Este token é usado para verificar se o usuário autenticado é a pessoa que realmente faz as solicitações ao aplicativo. Como esse token é armazenado na sessão do usuário e muda toda vez que a sessão é regenerada, um aplicativo malicioso não consegue acessá-lo

- O token CSRF da sessão atual pode ser acessado por meio da sessão da solicitação ou por meio da função auxiliar "csrf_token":

- Exemplo:
    ~~~php
        use Illuminate\Http\Request;

        Route::get('/token', function (Request $request) {
            $token = $request->session()->token();

            $token = csrf_token();

            // ...
        });
    ~~~

- Sempre que definir um formulário HTML "POST", "PUT", "PATCH" ou "DELETE" em seu aplicativo, você deve incluir um campo CSRF _token oculto no formulário para que o middleware de proteção CSRF possa validar a solicitação. Por conveniência, você pode usar a diretiva @csrf Blade para gerar o campo de entrada de token oculto:

- Exemplo:
    ~~~php
        <form method="POST" action="/profile">
            @csrf

            <!-- Equivalent to... -->
            <input type="hidden" name="_token" value="{{ csrf_token() }}" />
        </form>
    ~~~

- O middleware "App \ Http \ Middleware \ VerifyCsrfToken", que está incluído no grupo de middleware da web por padrão, verificará automaticamente se o token na entrada da solicitação corresponde ao token armazenado na sessão. Quando esses dois tokens coincidem, sabemos que o usuário autenticado é aquele que inicia a solicitação.

## *Controllers:*

- Os controladores são uma forma de organizar seu código agrupando a lógica de tratamento de solicitações relacionadas em uma única classe.

### **Writing Controllers(Escrevendo controllers):**

#### **Basic Controllers(Controllers básicos):**

- A classe base de todos os controller é "App\Http\Controllers\ontroller"

- Exemplo controller basico:
    ~~~php
        <?php
        namespace App\Http\Controllers;

        use App\Http\Controllers\Controller;
        use App\Models\User;

        class UserController extends Controller
        {
            /**
            * Show the profile for a given user.
            *
            * @param  int  $id
            * @return \Illuminate\View\View
            */
            public function show($id)
            {
                return view('user.profile', [
                    'user' => User::findOrFail($id)
                ]);
            }
        }
    ~~~

- Como definir um controller para uma rota:

    ~~~php
        use App\Http\Controllers\UserController;

        Route::get('/user/{id}', [UserController::class, 'show']);
    ~~~

#### **Single Action Controllers(Controladores de ação unica):**

- Se você acha que um método tem uma função muito extensa e portanto merece uma classe somente para ele, o Laravel tem uma solução com o método "__invoke":

- Exemplo:
    ~~~php
    <?php
        namespace App\Http\Controllers;

        use App\Http\Controllers\Controller;
        use App\Models\User;

        class ProvisionServer extends Controller
        {
            /**
            * Provision a new web server.
            *
            * @return \Illuminate\Http\Response
            */
            public function __invoke()
            {
                // ...
            }
        }
    ~~~

- Ao registrar rotas para controladores de ação única, você não precisa especificar um método de controlador. Em vez disso, você pode simplesmente passar o nome do controlador para o roteador:

- Exemplo:
    ~~~php
        use App\Http\Controllers\ProvisionServer;

        Route::post('/server', ProvisionServer::class);
    ~~~

### **Controller Middleware(Middleware de controlador):**

- Existem duas maneiras de usar um middleware juntamente com um controller, uma delas é através das rotas:

- Exemplo:
    ~~~php
        Route::get('profile', [UserController::class, 'show'])->middleware('auth');
    ~~~

- A outra maneira é definir o middleware no controller através do método middleware:

- Exemplo: 
    ~~~php
        class UserController extends Controller
        {
            __construct()
            {
                $this->middleware('auth');
                $this->middleware('log')->only('index');
                $this->middleware('subscribed')->except('store');
            }
        }
    ~~~

## *HTTP Requests:*

- O Laravel oferece uma maneira simples de interagir com as requisições HTTP através da sua class "Illuminate\Http\Request".

### **Interacting With The Request(Interagindo com a requisição):**

#### **Accessing The Request(Acessabdo a requisição):**

- Para acessar uma requisição no seu controller basta colocar a clausula "use" seguida do "Illuminate\Http\Request" e utilizar da classe "Request":

- Exemplo:
    ~~~php
        <?php

        namespace App\Http\Controllers;

        use Illuminate\Http\Request;

        class UserController extends Controller
        {
            /**
            * Store a new user.
            *
            * @param  \Illuminate\Http\Request  $request
            * @return \Illuminate\Http\Response
            */
            public function store(Request $request)
            {
                $name = $request->input('name');

                //
            }
        }
    ~~~

- Você também pode utilizar da "Request" em uma rota:

- Exemplo:
    ~~~php
        use Illuminate\Http\Request;

        Route::get('/', function (Request $request) {
            //
        });
    ~~~

#### **Request Path & Method(Solicitar caminho e método):**

- O método path presente no "Request" irá retornar o caminha da requisição, por exemplo em localhost:8000/ola/mundo, vai retornar ola/mundo:

- Exemplo:
    ~~~php
        $uri = $request->path();
    ~~~

#### **Inspecting The Request Path / Route (Inspecionando o Caminho / Rota):**

- O método "is" permite que você verifique se determinado caminho da solicitação corresponde a determinado padrão:

- Exemplo:
    ~~~php
        if ($request->is('admin/*')) {
            //
        }
    ~~~

- Caso você queira verificar uma rota nomeada, você pode fazer isso pelo método "routeIs":

- Exemplo:
    ~~~php
        if ($request->routeIs('admin.*')) {
            //
        }
    ~~~

#### **Retrieving The Request URL(Recuperando o URL da solicitação):**

- Para recuperar o URL completo da solicitação recebida, você pode usar os métodos "url" ou "fullUrl". O método url retornará o URL sem a string de consulta, enquanto o método fullUrl inclui a string de consulta:

- Exemplo:
    ~~~php
        $url = $request->url();

        $urlWithQueryString = $request->fullUrl();
    ~~~

#### **Retrieving The Request Method(Recuperando o Método de Solicitação):**

- Caso você queira obter qual o método Http foi chamado, basta utilizar o "method":

- Exemplo:
    ~~~php
        $method = $request->method();
    ~~~

- Caso você deseje fazer um condicional com o verbo, basta utilizar "isMethod":

- Exemplo:
    ~~~php
        if($request->isMethod("get"))
        {
            return "get";
        }
    ~~~

### **Request Headers:**

- Se você precisa recuperar o header para utilizar o método "header" presente em "Request". Se o header não estiver presente o método retornara null (Isso pode ser alterado passando um segundo parametro, que será o retorno caso não encontre o header):

- Exemplo:
    ~~~php
        $value = $request->header('X-Header-Name');

        $value = $request->header('X-Header-Name', 'default');
    ~~~

- Você também pode fazer uma comparação utilizando o método "hasHeader" para ver se o header existe:

- Exemplo:
    ~~~php
        if ($request->hasHeader('X-Header-Name')) {
        //
        }
    ~~~

- Por conveniencia o Laravel possui um método "bearerToken", que retorna o token da pessoa que está com header autorizado. Se o header não estiver presente será retornado uma string vazia:

- Exemplo:
    ~~~php
        $token = $request->bearerToken();
    ~~~

#### **Request IP Address(Requisição de indereço de ip):**

- O método "ip" pode ser usado para recuperar o ip que fez a requisição na sua aplicação:

- Exemplo:
    ~~~php
        $ip = $request->ip();
    ~~~

#### **Content Negotiation(Negociação de Conteúdo):**

- Para saber quais tipos são aceitos em uma determinada requisição o Laravel, nós diponibiliza o método "getAcceptableContentTypes()" (esse método retorna um array contendo todos os tipos aceitos pela requisição):

- Exemplo:
    ~~~php
        $contentTypes = $request->getAcceptableContentTypes();
    ~~~

- Caso você deseje saber se um determinado tipo é aceito na sua requisição, você também pode utilizar o método "accepts":

- Exemplo:
    ~~~php
        if ($request->accepts(['text/html', 'application/json'])) {
            // ...
        }
    ~~~

- Com o método "prefers", você pode determinar qual o tipo que terá maior prioridade na requisição:

- Exemplo:
    ~~~php
        $preferred = $request->prefers(['text/html', 'application/json']);
    ~~~

- OBS: Se nenhum dos tipos de conteúdo fornecidos for aceito pela solicitação, será retornado nulo;

- Se você quiser saber se determinada requisição aceita json você pode usar o método "expectsJson":

- Exemplo: 
    ~~~php
        if ($request->expectsJson()) {
            // ...
        }
    ~~~

### **Input:**

#### **Retrieving All Input Data(Recuperando todos os dados de input):**

- Você pode recuperar todos os dados dos inputs vindos da request como uma matriz usando o método "all". Este método pode ser usado independentemente de a solicitação de entrada ser de um formulário HTML ou uma solicitação XHR:

- Exemplo:
    ~~~php
        $input = $request->all();
    ~~~

#### **Retrieving An Input Value(Recuperando um unico valor de input):**

- Independentemente do verbo HTTP, o método de "input" pode ser usado para recuperar a entrada do usuário:

- Exemplo:
    ~~~php
        $name = $request->input('name');
    ~~~

- Você pode passar um valor padrão como segundo argumento do input:

- Exemplo:
    ~~~php
        $name = $request->input('name', 'Sally');
    ~~~

- Ao trabalhar com inputs de array, podemos usar o ponto para acessar o array:

- Exemplo:
    ~~~php
        $name = $request->input('products.0.name');

        $names = $request->input('products.*.name');
    ~~~

- Você pode chamar o input sem nenhum argumento, assim ele irá retornar todos os input como um array associativo:

- Exemplo:
    ~~~php
        $input = $request->input();
    ~~~

#### **Retrieving Input From The Query String(Recuperando input da string de consulta):**

- Enquanto o método de "input" recupera valores de toda a carga útil da solicitação (incluindo a string de consulta), o método de "query" recupera apenas os valores da string de consulta:

- Exemplo: 
    ~~~php
        $name = $resquest->query("name");
    ~~~

- Se os dados do valor da string de consulta solicitados não estiverem presentes, o segundo argumento para este método será retornado:

- Exemplo: 
    ~~~php
        $name = $request->query("name", "nomePadrao");
    ~~~

- Você pode chamar o método de "query" sem nenhum argumento para recuperar todos os valores da string de consulta como uma matriz associativa.

#### **Retrieving Boolean Input Values(Recuperando Valores Booleanos no input):**

- Quando trabalhamos com HTML podemos ter campos que retornam uma especie de boolean, sendo assim através do Laravel podemos pegar esses valores, como sendo realmente bool utilizando o método "boolean". O método booleano retorna verdadeiro para 1, "1", true, "true", "on" e "yes". Todos os outros valores retornarão falso:

- Exemplo:
    ~~~php
        $check = $request->boolean('checkbox');
    ~~~

#### **Retrieving a portion of the input data(Recuperando uma parte dos dados de entrada):**

- Se você precisar recuperar um subconjunto dos dados de entrada, pode usar os métodos "only" e "except". Ambos os métodos aceitam uma única matriz ou uma lista dinâmica de argumentos:

- Exemplo:
    ~~~php
        $input = $request->only(['username', 'password']);

        $input = $request->only('username', 'password');

        $input = $request->except(['credit_card']);

        $input = $request->except('credit_card');
    ~~~

### **Determining If Input Is Present(Determinando se o input está presente):**

- Você pode usar o método "has" para saber se um valor está presente dentro da requisição:

- Exemplo:
    ~~~php
        if ($request->has('name')) {
            //
        }
    ~~~

- Quando fornecido uma matriz, o método "has" determinará se todos os valores especificados estão presentes:

- Exemplo:
    ~~~php
        if ($request->has(['name', 'email'])) {
            //
        }   
    ~~~

- O método "whenHas" executará o fechamento fornecido se um valor estiver presente na solicitação:

- Exemplo:
    ~~~php
        $request->whenHas('name', function () {
            //
        });
    ~~~

- O método "hasAny" retorna verdadeiro se algum dos valores especificados estiver presente:

- Exemplo:
    ~~~php
        if ($request->hasAny(['name', 'email'])) {
            //
        }
    ~~~

- Se você deseja determinar se um valor está presente na requisição e não está vazio, você pode usar o método "filled":

- Exemplo:
    ~~~php
        if ($request->filled('name')) {
            //
        }
    ~~~

- O método "whenFilled" executará o fechamento fornecido se um valor estiver presente na requisição e não estiver vazio:

- Exemplo:
    ~~~php
        $request->whenFilled('name', function ($input) {
            //
        });
    ~~~

- Para determinar se uma determinada chave está ausente da solicitação, você pode usar o método "missing":

- Exemplo:
    ~~~php
        if ($request->missing('name')) {
            //
        }
    ~~~

### **Old Input(Input antigo):**

#### **Flashing Input To The Session(Entrada intermitente para a sessão):**

- O método "flash" na classe "Illuminate \ Http \ Request" atualizará a entrada atual para a sessão para que esteja disponível durante a próxima solicitação do usuário ao aplicativo:

- Exemplo: 
    ~~~php
        $request->flash();
    ~~~

### **Cookies:**

- Para acessar os valores dentro dos cookies com o laravel de forma simplificado é através do $request->cookie("<nome>");

## *HTTP Responses:*

### **Creating Responses(Criação de respostas):**

#### **Strings & Arrays:**

- O Laravel possui uma maneira diferente de retornas responses. A response mais básica retorna uma string de uma rota ou controller. O Framework vai transformar automaticamente a string para uma HTTP response:

- Exemplo:
    ~~~php
        Route::get('/', function () {
            return 'Hello World';
        });
    ~~~

- Caso você retorne um array o Laravel irá transforma-lo em uma response JSON:

- Exemplo:
    ~~~php
        Route::get('/', function () {
            return [1, 2, 3];
        });
    ~~~

#### **Response Objects(Objetos de Resposta):**

- Normalmente não iremos retornar apenas uma string ou uma matriz. Nós comumente retornamos uma instancia de "Response"(vem de Illuminate\Http\Response) ou uma view.

- Retornar uma instancia de "Response" permite que você personalize o código de status HTTP e os cabeçalhos. Uma instância Response é herdada da classe "Symfony \ Component \ HttpFoundation \ Response", que fornece uma variedade de métodos para construir respostas HTTP:

- Exemplo: 
    ~~~php
        Route::get('home', function () {
            return response('Hello World', 200)
                        ->header('Content-Type', 'text/plain');
        });
    ~~~

#### **Attaching Headers To Responses(Atribuindo headers para as reponses):**

- Para fazer essa atribuição basta utilizar o método header:

- Exemplo:
    ~~~php
        return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');
    ~~~

- OBS: Como é possível ver acima você pode usar um método após o outro sem problemas.

- Outra maneira de especificar multiplos métodos header é utilizando o "withHeaders", onde você passará um array de headers para adicionar a response:

- Exemplo:
    ~~~php
    return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
    ~~~

#### **Attaching Cookies To Responses(Atribuindo cookies às response)**:

- Com o método "cookie" o Laravel nós disponibiliza uma maneira simples e prática de atribuir um cookie a response:

- Exemplo:
    ~~~php
        return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);
    ~~~

- OBS: o método cookie do Laravel aceita mais atributos como o setcookie do PHP, porém são menos usados: "cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)"

- Uma alternativa para o método cookie é a classe Cookie com método estático queue, onde você pode criar um cookie que será anexado a response antes do navegador manda-lá:

- Exemplo:
    ~~~php
        Cookie::queue(Cookie::make('name', 'value', $minutes));

        Cookie::queue('name', 'value', $minutes);
    ~~~

### **Redirects(Redirecionamento):**

- As respostas de redirecionamento são instâncias da classe "Illuminate \ Http \ RedirectResponse" e contêm os cabeçalhos apropriados necessários para redirecionar o usuário para outro URL.

- Existem diversas maneiras de gerar a "RedirectResponse", porém o mais simples é o método global "redirect":

- Exemplo:
    ~~~php
        Route::get('dashboard', function () {
            return redirect('home/dashboard');
        });
    ~~~

- Às vezes, você pode querer redirecionar o usuário para o local anterior, como quando um formulário enviado é inválido. Para isso podemos utilizar o método "back":

- Exemplo:
    ~~~php
        Route::post('user/profile', function () {
            // Validate the request...

            return back()->withInput();
        });
    ~~~

#### **Redirecting To Controller Actions(Redirecionando para ações do controlador):**

- Para você redirecionar para um controller action, basta utilizar do método "action" e passar o nome da action:

- Exemplo:
    ~~~php
        return redirect()->action('HomeController@index');
    ~~~

- Caso seu controller route precise de parametros, basta passa-los como segundo parametro do método action:

- Exemplo:
    ~~~php
        return redirect()->action(
            'UserController@profile', ['id' => 1]
        );
    ~~~

#### **Redirecting To External Domains(Redirecionando para dominios externos):**

- Para fazer isso basta utilizar o método "away()":

- Exemplo:
    ~~~php
        return redirect()->away('https://www.google.com');
    ~~~

### **Other Response Types(Outros tipos de response):**

#### **View Responses:**

- Se você precisa retornar uma view, basta utilizar o método "view":

- Exemplo:
    ~~~php
        return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);
    ~~~

#### **JSON Responses:**

- Para retornar um Json de maneira pratica basta utilizar o método "json", que já irá setar o "Content-Type" para "application/json" e converter o array para um Json, usando o json_encode:

- Exemplo:
    ~~~php
        return response()->json([
            'name' => 'Abigail',
            'state' => 'CA',
        ]);
    ~~~

#### **File Downloads:**

- O método "download" pode ser usado para forçar que o navegador do usuario faça o download de um arquivo segundo o path.

- Exemplo:
    ~~~php
        return response()->download($pathToFile);

        return response()->download($pathToFile, $name, $headers);

        return response()->download($pathToFile)->deleteFileAfterSend();
    ~~~

- OBS: O método de download aceita um nome de arquivo como o segundo argumento para o método, que determinará o nome do arquivo que é visto pelo usuário que faz o download do arquivo. Finalmente, você pode passar uma matriz de cabeçalhos HTTP como o terceiro argumento para o método.

#### **File Responses:**

- O método de "file" pode ser usado para exibir um arquivo, como uma imagem ou PDF, diretamente no navegador do usuário, em vez de iniciar um download. Este método aceita o caminho para o arquivo como seu primeiro argumento e uma matriz de cabeçalhos como seu segundo argumento:

- Exemplo:
    ~~~php
        return response()->file($pathToFile);

        return response()->file($pathToFile, $headers);
    ~~~

### **Response Macros:**

- A reponse macro é uma maneira de você definir um padrão de reponse para utilizar em outras partes do seu programa.

## *Creating Views(Criando views):*

- As views são a parte HTML do seu programa, sendo separadas dos seus controllers / aplicação lógic.

- As views são armazenadas em "resources/views".

- O primeiro parametro da view é o seu nome, o segundo é uma array de argumentos que será passado para a view.

- Caso você tenha subdiretorios dentro de "resources/views" você pode utilizar o " . " para acessa-los:

- Exemplo:
    ~~~php
        return view('admin.profile', $data);
    ~~~

#### **Determining If A View Exists(Determinando se uma view existe):**

- Para saber se uma view existe, basta utilizar o método "exists", que retorna true caso exista:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\View;

        if (View::exists('emails.customer')) {
            //
        }
    ~~~

### **Passing Data To Views(Passando dados para as views):**

- Como já foi visto anteriormente é possível passar um array de dados para as views:

- Exemplo:
    ~~~php
        return view('greetings', ['name' => 'Victoria']);
    ~~~

- OBS: essas informações podem ser acessadas utilizando a chave como nome da variavel (Exemplo: $name).

- Uma alternativa para passar um array completo para a view, é utilizar o método "with"  para adicionar um dado individual:

- Exemplo:
    ~~~php
        return view('greeting')->with('name', 'Victoria');
    ~~~

#### **Sharing Data With All Views(Compartilhando dados com todas as views):**

- Ocasionalmente, você pode precisar compartilhar uma parte dos dados com todas as views que são renderizadas pelo seu aplicativo. Você pode fazer isso usando o método "share". Normalmente, você deve fazer chamada do "share" dentro do método "boot" de um provedor de serviços:

- Exemplo:
    ~~~php
        <?php

        namespace App\Providers;

        use Illuminate\Support\Facades\View;

        class AppServiceProvider extends ServiceProvider
        {
            /**
            * Register any application services.
            *
            * @return void
            */
            public function register()
            {
                //
            }

            /**
            * Bootstrap any application services.
            *
            * @return void
            */
            public function boot()
            {
                View::share('key', 'value');
            }
        }
    ~~~

### **View Composers:**

- As "view composers" são retornos de chamada ou métodos de classe que são chamados quando uma visualização é renderizada. Se você tiver dados que deseja vincular a uma visualização cada vez que essa visualização for renderizada, um compositor de visualização pode ajudá-lo a organizar essa lógica em um único local:

- Exemplo:
    ~~~php
        <?php

        namespace App\Providers;

        use Illuminate\Support\Facades\View;
        use Illuminate\Support\ServiceProvider;

        class ViewServiceProvider extends ServiceProvider
        {
            /**
            * Register any application services.
            *
            * @return void
            */
            public function register()
            {
                //
            }

            /**
            * Bootstrap any application services.
            *
            * @return void
            */
            public function boot()
            {
                // Using class based composers...
                View::composer(
                    'profile', 'App\Http\View\Composers\ProfileComposer'
                );

                // Using Closure based composers...
                View::composer('dashboard', function ($view) {
                    //
                });
            }
        }
    ~~~

- OBS:  o Laravel não inclui um diretório padrão para os compositores de visões. Você é livre para organizá-los como desejar. Por exemplo, você pode criar um diretório app / Http / View / Composers.

### **Optimizing Views:**

- Uma boa pratica para optimizar o seu código quando está em produção é utilizar o comando:

~~~php
    php artisan view:cache
~~~

- Esse comando irá limpar o cache das views, assim evitando lentidão, por conta das alterações de views.

- Você também pode usar o comando 'view:clear" para limpar o cache das views.

## *URL Generation:*

- O Laravel fornece vários helpers para ajudá-lo a gerar URLs para sua aplicação. Eles são úteis principalmente ao construir links em seus modelos e respostas de API, ou ao gerar respostas de redirecionamento para outra parte de seu aplicativo.

### **The Basic:**

#### **Generating Basic URLs:**

- Para gerar um URL básica, basta utilizar do helper "url".

- OBS: O URL gerado usará automaticamente o esquema (HTTP ou HTTPS) e o host da solicitação atual.

- Exemplo:
    ~~~php
        $post = App\Post::find(1);

        echo url("/posts/{$post->id}");

        // http://example.com/posts/1
    ~~~

#### **Accessing The Current URL(Acessando a URL atual):**

- Se nenhum parametro for passado para o método "url" ele irá retornar uma instancia de "Illuminate\Routing\UrlGenerator" permitindo que você acesse informações sobre o URL atual:

- Exemplo:
    ~~~php
        // Get the current URL without the query string...
        echo url()->current();

        // Get the current URL including the query string...
        echo url()->full();

        // Get the full URL for the previous request...
        echo url()->previous();
    ~~~

- Cada um desses métodos também pode ser acessado pela "facade" "URL":

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\URL;

        echo URL::current();
    ~~~

**...**

## *HTTP Session:*

- As sessões são uma maneira de armazenar informações em varias solicitações.

#### **Configuration:**

- O arquivo de configuração das sessões é armazenado em "config/session.php". Por padrão, o Laravel é configurado para usar o driver "file" de sessão, que funcionará bem para muitos aplicativos.

- A opção de configuração do "driver" de sessão define onde os dados da sessão serão armazenados para cada solicitação. O Laravel vem com vários drivers excelentes prontos para uso:
    - `file`: as sessões são armazenadas em "storage/framework/sessions".
    - `cookie`: as sessões são armazenadas em segurança, em um cookie encriptado;
    - `database`: as sessões são armazenadas em um banco de dados relacional;
    - `memcached / redis`: as sessões são armazenadas em um desses armazenamentos rápidos baseados em cache;
    - `array`: as sessões são armazenadas em um array PHP e não serão persistidas;

#### **Driver Prerequisites:**

- Tanto para o driver database e o redis são necessários pré-requisitos, saiba mais na docs oficial do laravel.

### **Using The Session:**

#### **Retrieving Data(Recuperando dados):**

- Existem duas maneiras de recuperar dados de sessões com o laravel:
    - Com o helper global "session";
    - E com a instancia do "Request";

- Vamos ver inicialmente a instancia do Request:

- Exemplo:
    ~~~php
        <?php

        namespace App\Http\Controllers;

        use App\Http\Controllers\Controller;
        use Illuminate\Http\Request;

        class UserController extends Controller
        {
            /**
            * Show the profile for the given user.
            *
            * @param  Request  $request
            * @param  int  $id
            * @return Response
            */
            public function show(Request $request, $id)
            {
                $value = $request->session()->get('key');

                //
            }
        }
    ~~~

- OBS: Existem meios de passar valores padrões caso as sessões não existam, eles são: passando segundo valor no método get, e passando uma função de fechamento.

#### **The Global Session Helper:**

- Você também pode utilizar das sessões através do helper "session" global, onde é possível passar um ou muitos parametros:

- Exemplo:
    ~~~php
        Route::get('home', function () {
        // Retrieve a piece of data from the session...
        $value = session('key');

        // Specifying a default value...
        $value = session('key', 'default');

        // Store a piece of data in the session...
        session(['key' => 'value']);
        });
    ~~~

#### **Retrieving All Session Data(Recuperando todos os dados da sessão):**

- Se você quiser recuperar todos os dados de uma sessão basta utilizar o método "all":

- Exemplo:
    ~~~php
        $data = $request->session()->all();
    ~~~

#### **Determining If An Item Exists In The Session(Determinando se um item existe na sessão):**

- Para saber se uma chave existe na sessão basta utilizar o método "has", retornara true se existir ou null se não:

- Exemplo:
    ~~~php
        if ($request->session()->has('users')) {
            //
        }
    ~~~

- Para determinar se um item está presente na sessão, mesmo se seu valor for nulo, você pode usar o método "exists":

- Exemplo:
    ~~~php
        if ($request->session()->exists('users')) {
            //
        }
    ~~~

#### **Storing Data(Armazenando dados):**

- Para armazenar dados basta utilizar o método "put" do helper session:

- Exemplo:
    ~~~php
        // Via a request instance...
        $request->session()->put('key', 'value');

        // Via the global helper...
        session(['key' => 'value']);
    ~~~

#### **Flash Data:**

- Com o método "flash" é possível manter os itens de uma sessão somente até a proxima requisição, e após isso são deletados:

- Exemplo:
    ~~~php
        $request->session()->flash('status', 'Task was successful!');
    ~~~

- Caso você queira manter os dados por mais uma requisição basta utilizar o método "reflash", e caso queira que alguns métodos permanessam, basta utilizar o "keep":

- Exemplo:
    ~~~php
        $request->session()->reflash();

        $request->session()->keep(['username', 'email']);
    ~~~

#### **Deleting Data(Deleção de dados):**

- O método "forget" remove um ou alguns dados de uma sessão. Já o método "flush" removerá todos:

- Exemplo:
    ~~~php
        // Forget a single key...
        $request->session()->forget('key');

        // Forget multiple keys...
        $request->session()->forget(['key1', 'key2']);

        $request->session()->flush();
    ~~~

### **Session Blocking:**

- É importante para melhor as requisições: porém não é essencial ta vá na docs do laravel.



