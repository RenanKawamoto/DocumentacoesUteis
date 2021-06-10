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

