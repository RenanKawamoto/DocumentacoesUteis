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


## *Validation:*

- O laravel oferece diferenes abordagens para validar os dados da sua aplicação. Porém o mais comum de ser utilizado é o método "validate" presente em todos os HTTP requests.

- O método validate irá tentar validar a sua request, seguindo algumas regras, caso passe seu código seguirá, caso não passe seu código irá redirecionar automaticamente o usuário de volta ao seu local anterior e gerar um erro que será mandado para a sessão.

### **Validation Quickstart:**

- Existem duas maneiras de utilizar o método "validate", sendo uma delas através de strings e outra através de arrays, veja seguir um exemplo:

- Exemplo:  
    ~~~php
        public function store(Request $request)
        {
            $validated = $request->validate([
                'title' => 'required|unique:posts|max:255',
                'body' => 'required',
            ]);
        }
    ~~~

- Como é possível ver no método store utilizamos o validate na $request, passando como parametros um array chave valor onde o "valor" é uma string com " | " para simbolizar os espaços.

- OBS: Não se preocupe com as regras passadas no valor, pois há uma explicação completa delas na documentação do laravel: https://laravel.com/docs/8.x/validation#available-validation-rules.

- A segunda maneira de se utilizar o validate é passando um array como valor:

- Exemplo:
    ~~~php
        $validatedData = $request->validate([
            'title' => ['required', 'unique:posts', 'max:255'],
            'body' => ['required'],
        ]);
    ~~~

- Caso você deseje também é possível criar um pacote de erro nomeado que irá guardar o erro gerado nas validações, através do "validateWithBag":

- Exemplo:
    ~~~php
        $validatedData = $request->validateWithBag('post', [
            'title' => ['required', 'unique:posts', 'max:255'],
            'body' => ['required'],
        ]); 
    ~~~

#### **Displaying The Validation Errors(Exibindo erros de validação):**

- Todos o erros gerados vão para uma variavel global entre as views(compartilhada pelo middleware Illuminate\View\Middleware\ShareErrorsFromSession, que é fornecido pelo grupo de middleware da web) chamada "$errors".

- A variável $errors será uma instância de "Illuminate \ Support \ MessageBag"

#### **Customizing The Error Messages:**

- Para customizar as mensagens de erros geradas pelas validações com o laravel é muito simples, bsta alterar em "resources/lang/en/validation.php".

#### **The @error Directive:**

- Você pode usar a diretiva @error do Blade para determinar rapidamente se existem mensagens de erro de validação para um determinado atributo. Dentro de uma diretiva @error, você pode usar a variável $message para exibir a mensagem de erro:

- Exemplo:
    ~~~php
        <!-- /resources/views/post/create.blade.php -->

        <label for="title">Post Title</label>

        <input id="title" type="text" name="title" class="@error('title') is-invalid @enderror">

        @error('title')
            <div class="alert alert-danger">{{ $message }}</div>
        @enderror
    ~~~

#### **Repopulating Forms:**

- Quando o Laravel, redireciona o devido ao validation ele cria automaticamente um flash input, assim para obter os antigos valores de uma request, basta utilizar o método old():

- Exemplo: 
    ~~~php
        title = $request->old('title');
    ~~~

- O blade também oferece um helper global "old", que retorna o valor se existir e retorna null se não:

- Exemplo:
    ~~~php
        <input type="text" name="title" value="{{ old('title') }}">
    ~~~

### **Form Request Validation:**

#### **Creating Form Requests:**

- As solicitações de formulário são classes de solicitação personalizadas que encapsulam sua própria lógica de validação e autorização. Para criar uma classe de solicitação de formulário, você pode usar o comando make: request Artisan da CLI:

~~~
    php artisan make:request StorePostRequest
~~~

- A classe de solicitação de formulário gerada será colocada no diretório "app / Http / Requests".

- Cada solicitação de formulário gerada pelo Laravel possui dois métodos: "authorize" e "rules".

- Como você deve ter adivinhado, o método de "authorize" é responsável por determinar se o usuário autenticado no momento pode executar a ação representada pela solicitação, enquanto o método de "rules" retorna as regras de validação que devem ser aplicadas aos dados da solicitação:

- Exemplo:
    ~~~php
        /**
        * Get the validation rules that apply to the request.
        *
        * @return array
        */
        public function rules()
        {
            return [
                'title' => 'required|unique:posts|max:255',
                'body' => 'required',
            ];
        }
    ~~~

- Para utilizar esse request form, basta chama-lo em vez do "Request" e chamar o método "validated":

- Exemplo:
    ~~~php
        /**
        * Store a new blog post.
        *
        * @param  \App\Http\Requests\StorePostRequest  $request
        * @return Illuminate\Http\Response
        */
        public function store(StorePostRequest $request)
        {
            // The incoming request is valid...

            // Retrieve the validated input data...
            $validated = $request->validated();
        }
    ~~~

- Se a validação falhar, uma resposta de redirecionamento será gerada para enviar o usuário de volta ao local anterior. Os erros também serão exibidos na sessão para que estejam disponíveis para exibição. 

#### **Authorizing Form Requests:**

- A classe de solicitação de formulário também contém um método "authorize". Nesse método, você pode determinar se o usuário autenticado realmente tem autoridade para atualizar um determinado recurso.

#### **Preparing Input For Validation:**

- Se você precisar preparar ou higienizar quaisquer dados da solicitação antes de aplicar suas regras de validação, você pode usar o método "prepareForValidation":

- Exemplo:
    ~~~php
        use Illuminate\Support\Str;

        /**
        * Prepare the data for validation.
        *
        * @return void
        */
        protected function prepareForValidation()
        {
            $this->merge([
                'slug' => Str::slug($this->slug),
            ]);
        }
    ~~~

## *Error Handling(Manipulação de erros):*

- Quando você inicia um novo projeto Laravel, o tratamento de erros e exceções já está configurado para você. A classe "App \ Exceptions \ Handler" é onde todas as exceções lançadas por seu aplicativo são registradas e, em seguida, processadas para o usuário.

### **Configuration:**

- A opção "debug" presente dentro de "config/app.php" determina quanta informação será exibida para o usuario. Por padrão ele respeita o valor contido na variavel global "APP_DEBUG" (que está armazenada no arquivo .env);

- OBS: Durante a fase de desenvolvimento é recomendavel que deixe o APP_DEBUG com true, e em produção como false.

### **The Exception Handler:**

#### **Reporting Exceptions:**

- Todas as exceções são tratadas pela classe "App \ Exceptions \ Handler". Esta classe contém um método de "register" onde você pode registrar relatórios de exceção personalizados e renderizar retornos de chamada.

- O relatório de exceção é usado para registrar exceções ou enviá-las a um serviço externo como Flare, Bugsnag ou Sentry. Por padrão, as exceções serão registradas com base em sua configuração de registro.

**...**

## *Database: Getting Started:*

- Quase todos os aplicativos da web modernos interagem com um banco de dados. O Laravel torna a interação com bancos de dados extremamente simples em uma variedade de bancos de dados suportados usando SQL puro, "fluent query builder" e o Eloquent ORM.

#### **Configuration:**

- A configuração dos serviços de banco de dados do Laravel está localizada no arquivo de configuração config / database.php de sua aplicação.

- Neste arquivo, você pode definir todas as suas conexões de banco de dados, bem como especificar qual conexão deve ser usada por padrão. A maioria das opções de configuração dentro deste arquivo são orientadas pelos valores das variáveis ​​de ambiente do seu aplicativo.

### **Running SQL Queries:**

- Depois de configurar sua conexão de banco de dados, você pode executar consultas usando a classe estatica "DB". Essa classe fornece métodos para cada tipo de consulta: "select, update, insert, deleter e statement.

#### **Running A Select Query:**

- Para executar uma consulta SELECT básica, você pode usar o método "select" na fachada "DB":

- Exemplo:
    ~~~php
        <?php

        namespace App\Http\Controllers;

        use App\Http\Controllers\Controller;
        use Illuminate\Support\Facades\DB;

        class UserController extends Controller
        {
            /**
            * Show a list of all of the application's users.
            *
            * @return \Illuminate\Http\Response
            */
            public function index()
            {
                $users = DB::select('select * from users where active = ?', [1]);

                return view('user.index', ['users' => $users]);
            }
        }
    ~~~

- O primeiro argumento passado para o método select é a consulta SQL, enquanto o segundo argumento são quaisquer ligações de parâmetro que precisam ser vinculadas à consulta. Normalmente, esses são os valores das restrições da cláusula where. A vinculação de parâmetros fornece proteção contra injeção de SQL.

- O método "select" sempre retornará uma matriz de resultados. Cada resultado dentro da matriz será um objeto PHP "stdClass" que representa um registro do banco de dados:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::select('select * from users');

        foreach ($users as $user) {
            echo $user->name;
        }
    ~~~

#### **Using Named Bindings(Usando ligações nomeadas):**

- Ao invés de usar "? "para representar seus vínculos de parâmetro, você pode executar uma consulta usando vínculos nomeados:

- Exemplo:
    ~~~php
        $results = DB::select('select * from users where id = :id', ['id' => 1]);
    ~~~

#### **Running An Insert Statement(Rodando um insert demonstrativo):**

- Para rodar um insert demonstrativo, você terá que usar o método "insert" da fachada "DB". Ele funciona igual ao método select:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        DB::insert('insert into users (id, name) values (?, ?)', [1, 'Marc']);
    ~~~

- #### **Running An Update Statement(Executando uma declaração de atualização):**

- O método "update" deve ser usado para atualizar os registros existentes no banco de dados. O número de linhas afetadas pela instrução é retornado pelo método:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $affected = DB::update(
            'update users set votes = 100 where name = ?',
            ['Anita']
        );
    ~~~

#### **Running A Delete Statement(Executando uma declaração de exclusão):**

- O método "delete" deve ser usado para excluir registros do banco de dados. Como a "update", o número de linhas afetadas será retornado pelo método:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $deleted = DB::delete('delete from users'); 
    ~~~

#### **Running A General Statement(Executando uma declaração geral):**

- Algumas instruções do banco de dados não retornam nenhum valor. Para esses tipos de operações, você pode usar o método "statement" na fachada "DB":

- Exemplo:
    ~~~php
        DB::statement('drop table users');
    ~~~

#### **Using Multiple Database Connections(Usando mutiplos conexões com o db):**

- Se a sua aplicação define múltiplas conexões em seu arquivo de configuração "config / database.php", você pode acessar cada conexão através do método "connection" fornecido pela fachada "DB".

- O nome da conexão passado para o método de conexão deve corresponder a uma das conexões listadas em seu arquivo de configuração "config / database.php":

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::connection('sqlite')->select(...);
    ~~~

#### **Listening For Query Events(Ouvindo eventos de consulta):**

**...**

## *Database: Query Builder:*

- O construtor de consultas de banco de dados do Laravel fornece uma interface conveniente e fluente para criar e executar consultas de banco de dados. Ele pode ser usado para realizar a maioria das operações de banco de dados em sua aplicação e funciona perfeitamente com todos os sistemas de banco de dados suportados pelo Laravel.

- O construtor de consultas Laravel usa ligação de parâmetros PDO para proteger sua aplicação contra ataques de injeção SQL. Não há necessidade de limpar ou higienizar strings passadas para o construtor de consultas como ligações de consulta.

### **Running Database Queries:**

#### **Retrieving All Rows From A Table(Recuperando todas as linhas de uma tabela):**

- Para isso você pode usar o método "table" presente em "DB". O método "table" retorna uma instância de "fluent query builder" da tabela fornecida, permitindo assim que você encadeie mais restrições na consulta e, finalmente, recupere os resultados usando o método "get":

- Exemplo:
    ~~~php
        <?php

        namespace App\Http\Controllers;

        use App\Http\Controllers\Controller;
        use Illuminate\Support\Facades\DB;

        class UserController extends Controller
        {
            /**
            * Show a list of all of the application's users.
            *
            * @return \Illuminate\Http\Response
            */
            public function index()
            {
                $users = DB::table('users')->get();

                return view('user.index', ['users' => $users]);
            }
        }
    ~~~

- O método get retorna uma instância de "Illuminate \ Support \ Collection" contendo os resultados da consulta, em que cada resultado é uma instância do objeto "stdClass" do PHP. Sendo assim você pode acessar o valor de cada coluna acessando a coluna como uma propriedade do objeto:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::table('users')->get();

        foreach ($users as $user) {
            echo $user->name;
        }
    ~~~

#### **Retrieving A Single Row / Column From A Table:**

- Se você só precisa recuperar uma única linha de uma tabela de banco de dados, pode usar o método "first" da fachada "DB". Este método retornará um único objeto stdClass:

- Exemplo: 
    ~~~php
        $user = DB::table('users')->where('name', 'John')->first();

        return $user->email;
    ~~~

- Se você não precisa de uma linha inteira, pode extrair um único valor de um registro usando o método "value". Este método retornará o valor da coluna diretamente:

- Exemplo:
    ~~~php
        $email = DB::table('users')->where('name', 'John')->value('email');
    ~~~

- Para recuperar uma única linha por seu valor de coluna id, use o método "find":

- Exemplo:
    ~~~php
        $user = DB::table('users')->find(3);
    ~~~

#### **Retrieving A List Of Column Values(Recuperando uma lista de valores da coluna):**

- Se desejar recuperar uma instância "Illuminate \ Support \ Collection" contendo os valores de uma única coluna, você pode usar o método "pluck".

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $titles = DB::table('users')->pluck('title');

        foreach ($titles as $title) {
            echo $title;
        }
    ~~~

- Outra funcionalidade do "pluck", é a capacidade de passarmos um segundo parametro, que também é um coluna, que será entendido como chave do valor:

- Exemplo:
    ~~~php
        $titles = DB::table('users')->pluck('title', 'name');

        foreach ($titles as $name => $title) {
            echo $title;
        }
    ~~~

#### **Chunking Results:**

- Caso você esteja trabalhando com um db muito grande, é interessante utilizar o chunks, uma vez que com ele você pode pegar um bloco de valores de cada vez, até finalizar todo o processo:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        DB::table('users')->orderBy('id')->chunk(100, function ($users) {
            foreach ($users as $user) {
                //
            }
        });
    ~~~

- OBS: se você deseja dar update nos resultados é melhor usar o método "chunkById".

#### **Streaming Results Lazily(Resultados de streaming preguiçosamente):**

- O método "lazy", é muito semelhante com o chunk, pois também irá executar a consulta em pedaços, porém em um unico fluxo, retornando uma "LazyCollection":

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        DB::table('users')->lazy()->each(function ($user) {
            //
        });
    ~~~

- OBS: O mesmo vale para o lazy, caso você deseje atualizar os dados é necessário utilizar o "lazyById".

#### **Aggregates:**

- O construtor de consultas também fornece uma variedade de métodos para recuperar valores agregados, como "count, max, min, avg e sum". Você pode chamar qualquer um desses métodos após construir sua consulta:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::table('users')->count();

        $price = DB::table('orders')->max('price');
    ~~~

- Claro, você pode combinar esses métodos com outras cláusulas para ajustar como seu valor agregado é calculado:

- Exemplo:
    ~~~php
        $price = DB::table('orders')
                ->where('finalized', 1)
                ->avg('price');
    ~~~

#### **Determining If Records Exist:**

- Em vez de usar o método "count" para determinar se existem registros que correspondam às restrições de sua consulta, você pode usar os métodos "exists" e "doesntExist":

- Exemplo:
    ~~~php
        if (DB::table('orders')->where('finalized', 1)->exists()) {
             // ...
        }

        if (DB::table('orders')->where('finalized', 1)->doesntExist()) {
            // ...
        }
    ~~~

### **Select Statements:**

#### **Specifying A Select Clause:**

- Nem sempre você deseja selecionar todas as colunas de uma tabela de banco de dados. Usando o método "select", você pode especificar uma cláusula "select" personalizada para a consulta:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::table('users')
            ->select('name', 'email as user_email')
            ->get();
    ~~~

- O método "distinct" permite que você force a consulta a retornar resultados distintos:

- Exemplo:
    ~~~php
        $users = DB::table('users')->distinct()->get();
    ~~~

- Se você já tem uma instância do construtor de consultas e deseja adicionar uma coluna à cláusula de seleção existente, pode usar o método "addSelect":

- Exemplo:
    ~~~php
        $query = DB::table('users')->select('name');

        $users = $query->addSelect('age')->get();
    ~~~

### **Raw Expressions(Expressões brutas):**

- Às vezes, você pode precisar inserir uma string arbitrária de consulta. Para criar uma expressão de string bruta, você pode usar o método "raw" fornecido pela fachada "DB":

- Exemplo:
    ~~~php
        $users = DB::table('users')
             ->select(DB::raw('count(*) as user_count, status'))
             ->where('status', '<>', 1)
             ->groupBy('status')
             ->get();
    ~~~

#### **Raw Methods:**

- Em vez de usar o método "DB::raw", você também pode usar os seguintes métodos para inserir uma expressão "raw" em várias partes de sua consulta.

- OBS: Lembre-se, o Laravel não pode garantir que qualquer consulta usando expressões brutas esteja protegida contra vulnerabilidades de injeção SQL.

- **selectRaw:**
    - O método "selectRaw" pode ser usado no lugar de "addSelect(DB::raw (...))". Este método aceita uma matriz opcional de ligações como seu segundo argumento:

    - Exemplo:
        ~~~php
            $orders = DB::table('orders')
                ->selectRaw('price * ? as price_with_tax', [1.0825])
                ->get();
        ~~~

- **whereRaw / orWhereRaw:**
    - Os métodos "whereRaw" e "orWhereRaw" podem ser usados ​​para injetar uma cláusula "where" bruta em sua consulta. Esses métodos aceitam uma matriz opcional de ligações como seu segundo argumento:

    - Exemplo:
        ~~~php
            $orders = DB::table('orders')
                    ->whereRaw('price > IF(state = "TX", ?, 100)', [200])
                    ->get();
        ~~~

- **havingRaw / orHavingRaw:**
    - Os métodos havingRaw e orHavingRaw podem ser usados ​​para fornecer uma string bruta como o valor da cláusula "having". Esses métodos aceitam uma matriz opcional de ligações como seu segundo argumento.

    - Exemplo:
        ~~~php
            $orders = DB::table('orders')
                ->select('department', DB::raw('SUM(price) as total_sales'))
                ->groupBy('department')
                ->havingRaw('SUM(price) > ?', [2500])
                ->get();
        ~~~

- **orderByRaw:**
    - The orderByRaw é um método que é usado para fornecer uma string bruta como para o "order by":

    - Exemplo:
        ~~~php
            $orders = DB::table('orders')
                ->orderByRaw('updated_at - created_at DESC')
                ->get();
        ~~~

- **groupByRaw:**
    - O método groupByRaw pode ser usado para fornecer uma string bruta como o valor da cláusula group by:

    - Exemplo:
        ~~~php
            $orders = DB::table('orders')
                ->select('city', 'state')
                ->groupByRaw('city, state')
                ->get();
        ~~~

### **Joins:**

#### **Inner Join Clause:**

- O query builder também pode ser usado para adicionar cláusulas de junção(joins) às suas consultas. Para realizar um "inner join" básica, você pode usar o método "join" em uma instância do construtor de consultas. 

- O primeiro argumento passado para o método de "join" é o nome da tabela à qual você precisa fazer a junção, enquanto os argumentos restantes especificam as restrições de coluna para a junção. Você pode até juntar várias tabelas em uma única consulta:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $users = DB::table('users')
                    ->join('contacts', 'users.id', '=', 'contacts.user_id')
                    ->join('orders', 'users.id', '=', 'orders.user_id')
                    ->select('users.*', 'contacts.phone', 'orders.price')
                    ->get();
    ~~~

#### **Left Join / Right Join Clause:**

- Se desejar realizar uma "left join" ou "right join" em vez de uma "inner join", use os métodos "leftJoin" ou "rightJoin". Esses métodos têm a mesma funcionalidade que o método 'join':

- Exemplo:
    ~~~php
        $users = DB::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();

        $users = DB::table('users')
                    ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
                    ->get();
    ~~~

#### **Advanced Join Clauses:**

- Você também pode especificar cláusulas de junção mais avançadas. Para fazer isso, passe um fechamento como segundo argumento para o método "join". Esse fechamento será um instancia de "Illuminate\Database\Query\JoinClause", o que permite especificar restrições na cláusula "join".

- Exemplo:  
    ~~~php
        DB::table('users')
        ->join('contacts', function ($join) {
            $join->on('users.id', '=', 'contacts.user_id')->orOn(...);
        })
        ->get();
    ~~~

#### **Subquery Joins:**

- Você pode usar os métodos "joinSub", "leftJoinSub" e "rightJoinSub" para unir uma consulta a uma subconsulta. 

- Cada um desses métodos recebe três argumentos: a subconsulta, seu alias da tabela e um encerramento que define as colunas relacionadas.

- Exemplo:
    ~~~php
        $latestPosts = DB::table('posts')
                   ->select('user_id', DB::raw('MAX(created_at) as last_post_created_at'))
                   ->where('is_published', true)
                   ->groupBy('user_id');

        $users = DB::table('users')
                ->joinSub($latestPosts, 'latest_posts', function ($join) {
                    $join->on('users.id', '=', 'latest_posts.user_id');
                })->get();
    ~~~

### **Unions:**

- O construtor de consultas também fornece um método conveniente para "unir" duas ou mais consultas. Por exemplo, você pode criar uma consulta inicial e usar o método "union" para uni-la com mais consultas:

- Exemplo:
    ~~~php
        use Illuminate\Support\Facades\DB;

        $first = DB::table('users')
                    ->whereNull('first_name');

        $users = DB::table('users')
                    ->whereNull('last_name')
                    ->union($first)
                    ->get();
    ~~~

- Além do método de união, o construtor de consulta fornece um método "unionAll". As consultas combinadas usando o método "unionAll" não terão seus resultados duplicados removidos. O método "unionAll" tem a mesma assinatura de método do método de união.

### **Basic Where Clauses:**

#### **Where Clauses:**

- Você pode usar o método "where" do construtor de consultas para adicionar cláusulas "where" à consulta. A chamada mais básica para o método where requer três argumentos. O primeiro argumento é o nome da coluna. O segundo argumento é um operador, que pode ser qualquer um dos operadores com suporte do banco de dados. O terceiro argumento é o valor a ser comparado ao valor da coluna.

- Exemplo:
    ~~~php
        $users = DB::table('users')
                ->where('votes', '=', 100)
                ->where('age', '>', 35)
                ->get();
    ~~~

- Por conveniência, se você quiser verificar se uma coluna é = para um determinado valor, você pode passar o valor como o segundo argumento para o método where. O Laravel irá assumir que você gostaria de usar o operador =:

- Exemplo:
    ~~~php
        $users = DB::table('users')->where('votes', 100)->get();
    ~~~

- Como dito antes é possível usar qualquer operador disponivel no sql:
    ~~~php
        $users = DB::table('users')
                ->where('votes', '>=', 100)
                ->get();

        $users = DB::table('users')
                        ->where('votes', '<>', 100)
                        ->get();

        $users = DB::table('users')
                        ->where('name', 'like', 'T%')
                        ->get();
    ~~~

- Você também pode passar uma série de condições para a função where. Cada elemento da matriz deve ser uma matriz contendo os três argumentos normalmente passados ​​para o método where:

- Exemplo:
    ~~~php
        $users = DB::table('users')->where([
            ['status', '=', '1'],
            ['subscribed', '<>', '1'],
        ])->get();
    ~~~

#### **Or Where Clauses:**

- Ao encadear chamadas para o método where do construtor de consultas, as cláusulas "where" serão unidas usando o operador "and". No entanto, você pode usar o método "orWhere" para associar uma cláusula à consulta usando o operador "or". O método orWhere aceita os mesmos argumentos do método where:

- Exemplo:
    ~~~php
        $users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();
    ~~~

- Se precisar agrupar uma condição "ou" entre parênteses, você pode passar um encerramento como o primeiro argumento para o método orWhere:

- Exemplo:
    ~~~php
        $users = DB::table('users')
            ->where('votes', '>', 100)
            ->orWhere(function($query) {
                $query->where('name', 'Abigail')
                      ->where('votes', '>', 50);
            })
            ->get();
    ~~~

#### **JSON Where Clauses:**

- O Laravel também suporta a consulta de tipos de coluna JSON em bancos de dados que fornecem suporte para tipos de coluna JSON. Para consultar uma coluna JSON, use o operador "->":

- Exemplo: 
    ~~~php
        $users = DB::table('users')
                ->where('preferences->dining->meal', 'salad')
                ->get();
    ~~~

- Você pode usar "whereJsonContains" para consultar matrizes JSON:

- Exemplo:
    ~~~php
        $users = DB::table('users')
                    ->whereJsonContains('options->languages', 'en')
                    ->get();
    ~~~

- Se seu aplicativo usa bancos de dados MySQL ou PostgreSQL, você pode passar uma matriz de valores para o método whereJsonContains:

- Exemplo:
    ~~~php
        $users = DB::table('users')
                ->whereJsonContains('options->languages', ['en', 'de'])
                ->get();
    ~~~

- Você pode usar o método whereJsonLength para consultar matrizes JSON por seu comprimento:

- Exemplo:
    ~~~php
        $users = DB::table('users')
                ->whereJsonLength('options->languages', 0)
                ->get();

        $users = DB::table('users')
                        ->whereJsonLength('options->languages', '>', 1)
                        ->get();
    ~~~

#### **Additional Where Clauses:**

- **whereBetween / orWhereBetween:**
    - O método whereBetween verifica se o valor de uma coluna está entre dois valores:
    
    - Exemplo:
        ~~~php
            $users = DB::table('users')
           ->whereBetween('votes', [1, 100])
           ->get();
        ~~~

- **whereNotBetween / orWhereNotBetween:**
    - O método whereNotBetween verifica se o valor de uma coluna está fora de dois valores:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                    ->whereNotBetween('votes', [1, 100])
                    ->get();
        ~~~

- **whereIn / whereNotIn / orWhereIn / orWhereNotIn:** 
    - O método "whereIn" verifica se o valor de uma determinada coluna está contido na matriz fornecida:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                    ->whereIn('id', [1, 2, 3])
                    ->get();
        ~~~

    - O método whereNotIn verifica se o valor da coluna fornecida não está contido na matriz fornecida:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                    ->whereNotIn('id', [1, 2, 3])
                    ->get();
        ~~~

- **whereNull / whereNotNull / orWhereNull / orWhereNotNull:** 
    - O método whereNull verifica se o valor da coluna fornecida é NULL:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereNull('updated_at')
                ->get();
        ~~~

    - O método whereNotNull verifica se o valor da coluna não é NULL:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereNotNull('updated_at')
                ->get();
        ~~~

- **whereDate / whereMonth / whereDay / whereYear / whereTime:**
    - O método whereDate pode ser usado para comparar o valor de uma coluna com uma data:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereDate('created_at', '2016-12-31')
                ->get();
        ~~~

    - O método whereMonth pode ser usado para comparar o valor de uma coluna com um mês específico:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereMonth('created_at', '12')
                ->get();
        ~~~

    - O método whereDay pode ser usado para comparar o valor de uma coluna com um dia específico do mês:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereDay('created_at', '31')
                ->get();
        ~~~

    - O método whereYear pode ser usado para comparar o valor de uma coluna com um ano específico:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereYear('created_at', '2016')
                ->get();
        ~~~

    - O método whereTime pode ser usado para comparar o valor de uma coluna com um tempo específico:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereTime('created_at', '=', '11:20:45')
                ->get();
        ~~~

- **whereColumn / orWhereColumn:**
    - O método whereColumn pode ser usado para verificar se duas colunas são iguais:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereColumn('first_name', 'last_name')
                ->get();
        ~~~

    - Você também pode passar um operador de comparação para o método whereColumn:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereColumn('updated_at', '>', 'created_at')
                ->get();
        ~~~

    - Você também pode passar uma matriz de comparações de coluna para o método whereColumn. Essas condições serão unidas usando o operador 'and':

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->whereColumn([
                    ['first_name', '=', 'last_name'],
                    ['updated_at', '>', 'created_at'],
                ])->get();
        ~~~

#### **Ordering, Grouping, Limit & Offset**

#### **Ordering:**

- **The orderBy Method:**
    - O método orderBy permite classificar os resultados da consulta por uma determinada coluna. O primeiro argumento aceito pelo método orderBy deve ser a coluna pela qual você deseja classificar, enquanto o segundo argumento determina a direção da classificação e pode ser asc ou desc:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->orderBy('name', 'desc')
                ->get();
        ~~~

    - OBS: você pode chamar quantas vezes forem necessárias esse método.

- **The latest & oldest Methods:**
    - Os métodos "latest" e "oldest" permitem ordenar facilmente os resultados por data. Por padrão, o resultado será ordenado pela coluna created_at da tabela. Ou você pode passar o nome da coluna que deseja classificar:

    - Exemplo:
        ~~~php
            $user = DB::table('users')
                ->latest()
                ->first();
        ~~~

- **Random Ordering:**
    - O método inRandomOrder pode ser usado para classificar os resultados da consulta aleatoriamente:

    - Exemplo:
        ~~~php
            $randomUser = DB::table('users')
                ->inRandomOrder()
                ->first();
        ~~~

- **Removing Existing Orderings:**
    - O método reordenar remove todas as cláusulas "order by" que foram aplicadas anteriormente à consulta:

    - Exemplo:
        ~~~php
            $query = DB::table('users')->orderBy('name');

            $unorderedUsers = $query->reorder()->get();
        ~~~

    - Você pode passar uma coluna e uma direção ao chamar o método reordenar para remover todas as cláusulas "order by" existentes e aplicar uma ordem inteiramente nova à consulta:

    - Exemplo:
        ~~~php
            $query = DB::table('users')->orderBy('name');

          $usersOrderedByEmail = $query->reorder('email', 'desc')->get();
        ~~~

#### **Grouping:**

- **The groupBy & having Methods:**
    - Como você pode esperar, os métodos groupBy e having podem ser usados ​​para agrupar os resultados da consulta. A assinatura do método "having" é semelhante à do método where:

    - Exemplo:
        ~~~php
            $users = DB::table('users')
                ->groupBy('account_id')
                ->having('account_id', '>', 100)
                ->get();
        ~~~

    - Você pode passar vários argumentos para o método groupBy para agrupar por várias colunas:

    - Exemplo: 
        ~~~php
            $users = DB::table('users')
                ->groupBy('first_name', 'status')
                ->having('account_id', '>', 100)
                ->get();
        ~~~

#### **Limit & Offset:**
    
- **The skip & take Methods:**
    - Você pode usar os métodos skip and take para limitar o número de resultados retornados da consulta ou para pular um determinado número de resultados na consulta:

    - Exemplo:
        ~~~php
            $users = DB::table('users')->skip(10)->take(5)->get();
        ~~~

    - Como alternativa, você pode usar os métodos de "limit e offset". Esses métodos são funcionalmente equivalentes aos métodos take e skip, respectivamente:

### **Conditional Clauses:**

- Às vezes, você pode querer que certas cláusulas de consulta se apliquem a uma consulta com base em outra condição. Você pode fazer isso usando o método "when":

- Exemplo:
    ~~~php
        $role = $request->input('role');

        $users = DB::table('users')
                        ->when($role, function ($query, $role) {
                            return $query->where('role_id', $role);
                        })
                        ->get();
    ~~~

- O método when apenas executa o fechamento fornecido quando o primeiro argumento é verdadeiro. Se o primeiro argumento for falso, o fechamento não será executado.

- Você pode passar outro encerramento como o terceiro argumento para o método when. Este fechamento só será executado se o primeiro argumento for avaliado como falso.

- Exemplo:
    ~~~php
        $sortByVotes = $request->input('sort_by_votes');

        $users = DB::table('users')
                        ->when($sortByVotes, function ($query, $sortByVotes) {
                            return $query->orderBy('votes');
                        }, function ($query) {
                            return $query->orderBy('name');
                        })
                        ->get();
    ~~~

### **Insert Statements:**

- O construtor de consultas também fornece um método de inserção que pode ser usado para inserir registros na tabela do banco de dados. O método "insert" aceita uma matriz de nomes e valores de coluna:

- Exemplo:
    ~~~php
        DB::table('users')->insert([
            'email' => 'kayla@example.com',
            'votes' => 0
        ]);
    ~~~

- Você pode inserir vários registros de uma vez, passando uma matriz de matrizes. Cada array representa um registro que deve ser inserido na tabela:

- Exemplo:
    ~~~php
        DB::table('users')->insert([
            ['email' => 'picard@example.com', 'votes' => 0],
            ['email' => 'janeway@example.com', 'votes' => 0],
        ]);
    ~~~

#### **Auto-Incrementing IDs:**

- Se a tabela tiver um ID de incremento automático, use o método insertGetId para inserir um registro e, em seguida, recuperar o ID:

- Exemplo:
    ~~~php
        $id = DB::table('users')->insertGetId(
            ['email' => 'john@example.com', 'votes' => 0]
        );
    ~~~

#### **Upserts:**

- O método upsert irá inserir registros que não existem e atualizar os registros que já existem com novos valores que você pode especificar. O primeiro argumento do método consiste nos valores a inserir ou atualizar, enquanto o segundo argumento lista as colunas que identificam exclusivamente os registros na tabela associada. O terceiro e último argumento do método é uma matriz de colunas que deve ser atualizada se um registro correspondente já existir no banco de dados:

- Exemplo:
    ~~~php
            DB::table('flights')->upsert([
            ['departure' => 'Oakland', 'destination' => 'San Diego', 'price' => 99],
            ['departure' => 'Chicago', 'destination' => 'New York', 'price' => 150]
        ], ['departure', 'destination'], ['price']);
    ~~~

### **Update Statements:**

- Além de inserir registros no banco de dados, o construtor de consultas também pode atualizar os registros existentes usando o método "update". O método "update", como o método "insert", aceita uma matriz de pares de coluna e valor indicando as colunas a serem atualizadas. Você pode restringir a consulta de atualização usando cláusulas where:

- Exemplo:
    ~~~php
        $affected = DB::table('users')
              ->where('id', 1)
              ->update(['votes' => 1]);
    ~~~

#### **Update Or Insert:**

- Às vezes, você pode querer atualizar um registro existente no banco de dados ou criá-lo se não houver nenhum registro correspondente. Nesse cenário, o método updateOrInsert pode ser usado. O método updateOrInsert aceita dois argumentos: uma matriz de condições pelas quais irá localizar o registro e uma matriz de pares de coluna e valor indicando as colunas a serem atualizadas.

- Exemplo:
    ~~~php
        DB::table('users')
            ->updateOrInsert(
                ['email' => 'john@example.com', 'name' => 'John'],
                ['votes' => '2']
            );
    ~~~

#### **Increment & Decrement:**

- O construtor de consultas também fornece métodos convenientes para aumentar ou diminuir o valor de uma determinada coluna. Ambos os métodos aceitam pelo menos um argumento: a coluna a ser modificada.Um segundo argumento pode ser fornecido para especificar o valor pelo qual a coluna deve ser incrementada ou diminuída:

- Exemplo:
    ~~~php
        DB::table('users')->increment('votes');

        DB::table('users')->increment('votes', 5);

        DB::table('users')->decrement('votes');

        DB::table('users')->decrement('votes', 5);
    ~~~

### **Delete Statements:**

- O método de "delete" do construtor de consultas pode ser usado para excluir registros da tabela. Você pode restringir as instruções de exclusão adicionando cláusulas "where" antes de chamar o método delete:

- Exemplo:
    ~~~php
        DB::table('users')->delete();

        DB::table('users')->where('votes', '>', 100)->delete();
    ~~~

## *Migrations:*

- Migration é um recurso de versionamento de banco de dados que é nativo do laravel.

### **Configurando o banco de dados:**

- Grande parte das configurações relacionada ao seu banco está contida no arquivo ".env"(compo por exemplo: DB_CONNECTION, DB_HOST, DB_PORT, DB_DATABASE, DB_USERNAME, DB_PASSWORD)

### **Diretorio das migrations:**

- As migrations ficam em "database/migrations".

### **Up e Down:**

- As suas migrations quando forem criadas viram por padrão com esses métodos: onde dentro do método Up é a criação/alteração dos recursos do banco, já o down é o rollback (sendo assim, tudo que você fizer no up, você terá que fazer o inverso no down).

### **Comando básico para criar a migration:**

~~~
    php artisan make:migration <nomeDaMigration>
~~~

### **Padronização de nomeclatura:**

- Ao criar uma migration de create table você deve seguir os seguintes padrões:
    - Coloque o termo "create_" e o nome(sempre em ingles);
    - Após o nome da migration coloque um "_table";
    - Coloque o nome da migration no plural;

    - Exemplo:
        ~~~php
            php artisan make:migration create_people_table
        ~~~

    - OBS: uma alternativa seria "php artisan make:migration <nomeDaMigration> --create <nomeDaTabela>"

- Ao criar uma migration de alter table você deve seguir os seguintes padrões:
    - Coloque o termo "add_" + "nomeDaColuna" + "_to_" + "nomeDaTabelaAdicionada";

    - Exemplo:
        ~~~php
            php artisan make:migration add_name_to_people
        ~~~

    - OBS: uma alternativa seria "php artisan make:migration <nomeDaMigration> --table <nomeDaTabelaAlterada>

### **Comandos migrate:**

- Rodar todas as migration pendentes:
    ~~~php
        php artisan migrate
    ~~~

- Voltar as migrations:
    ~~~php
        php artisan migrate:rollback <--step <n°PassoParaSeremVoltados>>
    ~~~

