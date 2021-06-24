# **Documentação básica sobre Laravel**

## *Sumário:*
1. 

## *O que é Laravel?*

- É um framework construido na linguagem PHP, que utiliza a arquitetira MVC e possui diversos recursos interessantes para auxiliar no desenvolvimento de aplicações, como por exemplo: artisan, migrations, blades e etc.

## *Como intalar o laravel?*

### **No linux(com composer):**

Sintaxe:
~~~
    composer create-project    laravel/laravel <nomeDoProjeto>
~~~

Exemplo:
~~~
    composer create-project laravel/laravel projeto1
~~~

## *Como iniciar um server local com laravel artisan:*

- Com a porta padrão(8000):
    ~~~
        php artisan serve
    ~~~

- Com a porta alterada:
    ~~~
        php artisan serve --port=8091
    ~~~

## *Rotas e Views:*

- Rotas: são as URLs que nos redirecionam para uma view;

- View: são as representações graficas das páginas;

### **Onde ficam as rotas?**

- As rotas dentro do Laravel são encontradas em: **routes**

### **Onde ficam as views?**

- As view dentro do Laravel se encontram em: **resources >> views**

### **Como criar uma view:**

- Vá em: resources >> views e crie um arquivo novo ("nomeDaView.blade.php")

### **Como criar uma rota simples:**

- Vá em: routes >> web.php:

Sintaxe:
~~~php
    Route::<verbo>('/<uri>', function()
    {
        return view('<nomeDaView>');
    })
~~~

Exemplo:
~~~php
    Route::get('/teste', function()
    {
        return view('teste');
    })
~~~

### **Como passar variaveis pelas rotas**

- Para isso basta passar no return um array com as informações, veja a seguir:

- Sintaxe:
    ~~~php
        Route::<verbo>('/<uri>', function()
        {
            $<variavel> = <valorDaVariavel>;

            return view('<nomeDaView>', ['nomeDaVariavelQueIraApareceParaAViewBlade' => $<variavel>]);
        })
    ~~~

- Exemplo:
    ~~~php
        Route::get('/teste', function()
        {
            $n = 10;

            return view('welcome', ['numero' => $n]);
        })
    ~~~

## *O que é o Blade:*

- Blade é um template engine (é uma maneira de facilitar a criação de páginas HTML e tornar o envio e exibição de informações para estas páginas um processo mais simples e organizado) do laravel;

- Com ele é possível deixar as nossas views dinâmicas(inserindo tags HTML e dados que são fornecidos pelo banco);

### **Como usar o blade:**

- Para usar o blade é necessário utilizar um formatação expecifica com o "@", veja a seguir:

- Sintaxe:
    ~~~html
        @if(<condicional>)
        <codigoHTML>
        @endif
    ~~~

- Exemplo:
    ~~~html
        @if(10 < 5)
        <h1>Isso esta errado</h1>
        @else
        <h1>10 é maior</h1>
        @endif
    ~~~

### **Como pegar variaveis pelo blade:**

OBS: essas variaveis devem ser passadas pela rota.

- Sintaxe:
    ~~~html
        <h1>{{$<variavel>}}</h1>
    ~~~

- Exemplo:
    ~~~html
        <h1>{{$numero}}</h1>
    ~~~

### **Como usar o php puro pelo blade:**

- Exemplo:
    ~~~php
        @php
            echo 'ola mundo';
        @endphp
    ~~~

### **Como usar comentario pelo blade:**

- Sintaxe:
    ~~~
        {{-- Aqui fica o comentario --}}
    ~~~

OBS: Esse comentario é melhor do que o do html, pois não é mostrado no f12.

### **Layouts com Blade:**

- Os layouts tem a função reproveitamento de código, como por exemplo: header, footer, titles e etc;

- Para utilizarmos os layouts é necessário inicialmente criarmos seções com "@yield()", veja a seguir:

- Sintaxe:
    - Código do Layout:
        ~~~
            <html>
                <head>
                    <title>@yield('titulo')</title>
                </head>
                <body>
                    @yield('conteudo')
                    <header>Esse é o header</header>
                </body>
            </html>
        ~~~

    - Código que extende o layout:
        ~~~
            @extends('<pasta>.<nomeDoArquivo>')
            @section('<nomeDoYield>', <valor>)
            ...
            @endsection
        ~~~

## *Adicionando arquivos estáticos:*

OBS: Arquivos estáticos são, js, css e imgs

- Todos os recursos ficam na pasta public, e tem acesso direto nas tags que trabalham com arquivos estáticos.

## *Resgatando parametros de URL:*

### **Como exigir um parametro para um determinada rota:**

- Para isso é necessario ir até a rota que deseja e alterar a rota da seguinte forma:

- Sintaxe:
    ~~~
        Route::get('/<rota>/{<parametroObrigatorio>}, function($<parametroQueSeráPassado>)
        {
            return view('<nomeDaView>', ['variavel' => $<parametroQueSeráPassado>]);
        })
    ~~~

- Exemplo:
    ~~~
        Route::get('/teste/{id}, function($id)
        {
            return view('teste', ['id' => $id]);
        })
    ~~~

### **Como pedir parametro(opcional) para um determinada rota:**

- Sintaxe:
    ~~~
        Route::get('/<rota>/{<parametroObrigatorio>?}, function($<parametroQueSeráPassado> == null)
        {
            return view('<nomeDaView>', ['variavel' => $<parametroQueSeráPassado>]);
        })
    ~~~

- Exemplo:
    ~~~
        Route::get('/teste/{id?}, function($id == null)
        {
            return view('teste', ['id' => $id]);
        })
    ~~~

### **Como pegar parametros da url no padrão comum(?parametro=valor):**

- Para isso é necessário utilizar um função dentro da function que retorna sua view(a da rota):

- Sintaxe:
    ~~~
        Route::get('/<rota>', function()
        {
            $<variavel> = request('<nomeDoParametro>');

            return view('<nomeDaView>', ['<valorQueSeraPassadoParaView>'=> $<variavel>])
        })
    ~~~

- Exemplo:
    ~~~
        Route::get('/teste', function()
        {
            $nome = request('nome');

            return view('testeV', ['name'=> $nome])
        })
    ~~~

## *Controllers:*

- Os controllers são parte fundamental de toda aplicação em Laravel (geralmente neles que ficam a maior parte da lógica);

- Eles tem o papel de enviar e esperar resposta do banco de dados, além de receber e enviar algumas resposta para as views;

- Os controllers podem ser criados via artisan;

### **Onde ficam os controller:**

- Os controllers ficam localizados em: **app >> Http >> Controllers**

### **Qual é classe que todo controller deve extender:**

- Todos os controller devem herdar da classe Pai BaseController

### **Como criar um controller através do artisan:**

~~~
    php artisan make:controller <nomeDoController>
~~~

### **O que deve conter seu controller:**

- Dentro do controller deve ficar toda a lógica(tirando a relacionada com o db), e os redirecionamentos para as views.

OBS: tudo isso deve ser feito dentro de métodos, para funcionar.

### **Como deve ficar a sua rota ao usar o controller:**

Sintaxe:
~~~
    Route::<verbo>('<Uri>', [<NomeDoController>::class, '<nomeDoMétodo>']); 
~~~

Exemplo:

~~~
    Route::get('/', [MainPage::class, 'Index']);
~~~

## *Conexão com o banco de dados:*

### **Onde fazer as configurações do banco de dados:**

- No Laravel existe um arquivo especializado para armazenar as configurações do banco de dados, e esse é o `.env`.

### **O que o laravel utiliza para auxiliar no desenvolvimento do banco:**

- O Laravel usa o paradigma ORM (Object-Relational Mapping (ORM), em português, mapeamento objeto-relacional, é uma técnica para aproximar o paradigma de desenvolvimento de aplicações orientadas a objetos ao paradigma do banco de dados relacional.) - Eloquent.

- E para fazer a criação das tabelas o Laravel, utiliza `migrations`, que se trata de uma ferramenta para criar o db com o código em php já os versionando;

- **Comando php artisan migration:**

    - Esse comando tem como função de gerar todo o conteudo que estiver dentro da pasta migration (presente em database >> migration)

    - Sintaxe:
        ~~~
            php artisan migration
        ~~~

### **Migration:**

- Funcionam como um versionamento de banco de dados, sendo assim podemos avançar ou retroceder a qualquer momento;

- Podemos com elas fazer o setup de DB de uma nova instalação com apenas um comando;

- **Como criar uma migration:**
    - Para criar uma migration basta utilizar o artisan da seguinte maneira:

        - Sintaxe:
            ~~~
                php artisan make:migration create_<nomeDaMigration>
            ~~~

- **Estrutura básica de uma migration:**

~~~php
    class Teste extends Migration
    {
        public function up() 
        {
            Schema::create('teste1', function (Blueprint $table) {
                $table->id("cod");
            }); // criação de uma tabela teste1, com o campo cod(primary key)
        }
        public function down()
        {
            Schema::dropIfExists('teste1'); //dropa a tabela teste1
        }
    }
~~~

- **Comando para rodar TODAS as migrations novamente(deletando todos os dados da tabela):**

~~~
    php artisan migrate:fresh
~~~

- **Comando para ver o estato de suas migations:**

~~~
    php artisan migrate:status
~~~

- **Como adicionar uma nova coluna a uma tabela já existente:**

~~~
    php artisan make:migration add_<NomeDaColuna>_to_<NomeDaTabela>
~~~

OBS: com esse comando será criado um código que usa a tabela como uma base, para fazer o comando.

Após criar essa nova migration use o comando: "php artisan migrate".

- **Como voltar uma alteração com o rollback:**

OBS: Ao utilizar o rollback o laravel irá executar o código presente na function "down" da ultima migrate executada.

Sintaxe:
~~~
    php artisan migrate:rollback
~~~

- **Como resetar todos os migrates registrados até o momento:**

~~~
    php artisan migrate:reset
~~~

- **Como fazer o roolback+migration com apenas um comando:**

~~~
    php artisan migrate:refesh
~~~

### **Eloquent:**

- Eloquent é a ORM do Laravel;

- Cada table tem um Model que é responsável pela interação entre as requisições ao banco. Assim para converter um tabela para um Model temos uma boa prática, que é colocar a entidade no singular e com inicial maiuscula(sempre em ingles);

- **Onde ficam as models:**

    - As models ficam em "app >> Models"

- **Como criar um Model com artisan:**

~~~
    php artisan make:model <nomeDaModel>
~~~

- **Como pegar todos os dados de uma tabela:**

- Sintaxe:
    ~~~php
        use App\Models\<nomeDaModel>;

        class <nomeDoController> extends Controller
        {
            public function <nomeDaFuncao>()
            {
                $<variavel> = <nomeDaModel>::all();
                return view('<nomeDaView>', ['variavelEnviada'=>$<variavel>]);
            }
        }
    ~~~

- Exemplo:
    ~~~php
        use App\Models\Person;

        class MainPage extends Controller
        {
            public function Index()
            {
                $people = Person::all();
                return view('welcome',['people'=>$people]);
            }
        }
    ~~~

OBS: Caso você utilize a padronização a ORM irá encontrar sua tabela tranquilamente.

### **Como salvar dados dentro de uma tabela do banco, usando o Laravel:**

- Para salvar dados é necessário, ter uma rota apropriada para receber requisições post, um formulario e um método "store" no seu controller em questão (o nome store é apenas uma convenção).

- Rota para receber requisição post:
~~~php
    Route::post('/', [MainPage::class, 'store']);
~~~

- Formulario exemplo:
~~~html
    <form action="/" method = "POST">
        @csrf
        <input type="text" name='name' placeholder='nome'></input>
        <input type="text" name='age' placeholder='idade'></input>
        <button type='submit'>Enviar</button>
    </form>
~~~

OBS: é necessário utilizar o `@csrf` para que o laravel permitir as requisições post

- Método store:
~~~php
    use App\Models\Person;
    public function store(Request $request)
    {
        $person = new Person;
        $person->name = $request->name;
        $person->age = $request->age;
        $person->save();
        return redirect('/');
    }
~~~

Como é possível ver nesse exemplo, primeiramente é necessário intanciar o seu Model, e com o parametro Request mandar os campos do formulario, após isso é necessário usar a função save() para salvar os dados na tabela.

### **Flash Message:**

- As flash message são mensagens enviadas por sessões, que tem o intuito de dar um feedback para o usuario;

- **Como definir uma flash message no seu controller:**

Dentro do seu **controller** ao você usar o redirect(), basta chamar o método with(), onde você passa o nome da flash message e o conteudo dela:

~~~
    return redirect('/')->with('msg', 'Teste');
~~~

Já dentro da view é possível identifica-la pela diretiva @session(<nomeDaFlashMessage>):

~~~
    @if(session('msg'))
        {{session('msg')}}
    @endif
~~~

### **Salvando "Json" no banco:**

Para salvar dados como json(array), dentro do banco inicialmente precisamos de algumas coisas:

- Uma coluna do seu banco de dados, que contenha o tipo json:

    - Exemplo:
        ~~~
            Schema::table('people', function (Blueprint $table) {
                $table->json("events");
            });
        ~~~

- Um input na sua view que tenha um "name" de array:

    - Exemplo:
        ~~~
            <input type="checkbox" name='events[]' values='bar'>Open bar</input>
        ~~~

- E um cast no Model em questão:

    - Exemplo:
        ~~~
            protected $casts = [
                'events' => 'array',
            ];
        ~~~

### **Salvando datas com laravel:**

Para salvar datas no banco de dados com o Laravel é muito simples, basta ter:

- Um formulario com um input do type = date;

- E um campo no seu banco do tipo dateTime;

Após basta fazer o mesmo processo para salvar o dado, que qualquer outro.

- OBS:Caso deseje mostrar na view a data formatada basta utilizar o date("d/m/y", strtodate(<campoDoBanco>));

### **Como utilizar o where com laravel:**

Para utilizar o where em laravel basta instanciar o seu model e utilizar o método estatico where em vez de all:

- Sintaxe:
~~~php
    $people = <Model>::where([
        [<colunaDaTabela>, <LikeOpcional> , <DadoQueFicaADireitaDoWhere>]
    ])->get();
~~~

- Exemplo:
~~~php
    $search = request("search");    
    if($search)
    {
        $people = Person::where([
            ['name', $search]
        ])->get();
    }
    else
    {
        $people = Person::all();
    }
    return view('mainPage', ['people'=>$people]);
~~~

### **Como criar relacionamentos(one to many) com o laravel:**



