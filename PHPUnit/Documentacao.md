# **Documentação básica sobre PHPUnit:**

## *Sumário:*
1. [Instalação](#instalacao)
2. [Nomeclatura](#nomeclatura)
3. [Criando seu primeiro teste unitário](#primeiro-teste)
4. [Um pouco sobre os asserts](#asserts)
5. [Esperando excpetions](#exception)
6. [Mocks](#mocks)
7. [Testes com métodos sem retorno](#sem-retorno)


<div id='instalacao'></div>

## *- Instalação:*

- Pelo composer:
    
    ~~~
    composer require --dev phpunit/phpunit 
    ~~~

<div id='nomeclatura'></div>

## *- Entendo a nomeclatura dos testes unitários:*

- Ao começar a criar os testes unitários com o PHPUnit é interessante aprender a nomeclatura padrão utilizada pela comunidade:
    - `tests`: Diretorio padrão onde se encontram os testes.
    - `nomeDaClasseTest`: A classe deve possuir o sufixo Test.
    - `testFunction`: Já os métodos deve possuir o préfixo test.

> Uma alternativa para o prefixo test*: é possível identificar os testes por anotações (/**@test*/)

<div id='primeiro-teste'></div>

## *- Criando o seu primeiro teste unitário:*

- Nessa sessão irei explicar de maneira simplificada como escrever um teste unitário em sua aplicação:

> OBS: No exemplo utilizarei autoload(psr-4) do composer

- Modelo de diretorios para o exemplo:
    - src
        - Sum.php
    - tests
        - SumTest.php
    - vendor
        - ...
    - composer.json
    - composer.lock


- Modelo do composer.json:
    ~~~composer.json
    {
        "require": {
            "phpunit/phpunit": "^9.5"
        },
        "autoload": {
        "psr-4": {
            "App\\": "src/"
            }
        }
    }
    ~~~

- Arquivo Sum.php:

    ~~~php
    <?php
    namespace App;

    class Sum{
        public function sum(int $n, int $n2):int
        {
            try
            {
                return $n + $n2;
            }catch(\Exception $ex){
                return $ex;
            }
        }
    }
    ~~~

- Arquivo SumTest.php:

    ~~~php
    <?php

    require __DIR__."/../vendor/autoload.php";
    use App\Sum;
    use PHPUnit\Framework\TestCase;

    class SumTest extends TestCase
    {
        public function testShouldReturnSum()
        {
            $sum = new Sum();
            $result = $sum->sum(10, 50);
            
            $this->assertEquals(60, $result);
        }
    }
    ~~~

> OBS: Como é possível ver na classe SumTest, as classes de test extendem a classe base `TestCase`, assim é possível ter acesso aos recursos do PhpUnit, como por exemplo os `asserts` que serão melhor explicados a frente.

> OBS2: Não esqueça de usar o "comando composer dumpautoload".

- Para rodar o seu teste basta utilizar o binario que o PHPUnit nos disponibiliza:
    ~~~
    ./vendor/bin/phpunit tests/SumTest.php --colors
    ~~~

> OBS: O --colors é um parametro opcional que tem a intenção de tornar a visualização mais agradavel.

<div id='asserts'></div>

## *- Entendo melhor sobre os asserts:* 

- Os asserts é uma comparação entre uma afirmação e o que realmente está sendo retornado, resumindo é o que você espera de uma determinada função e o que realmente ocorre (caso ambos sejam iguais o teste se da como correto).

<div id='exception'></div>

## *- Esperando exceptions:*

- Existem alguns testes onde desejamos que uma exceção seja lançada para que esteja correto, assim sendo o PHPUnit nos disponibiliza o métodos: `expectException`, segue um exemplo de uso:

> OBS: a arquitetura de pastas e dependencias será a mesma do ultimo exemplo.

- O novo SumTest:
~~~php
<?php

require __DIR__."/../vendor/autoload.php";
use App\Sum;
use PHPUnit\Framework\TestCase;

class SumTest extends TestCase
{
    public function testShouldReturnSum()
    {
        $sum = new Sum();
        $result = $sum->sum(10, 50);
        
        $this->assertEquals(60, $result);
    }

    public function testShouldReturnExceptionWhenNotNumeric()
    {
        $sum = new Sum();
        $this->expectException(\TypeError::class);
        $result = $sum->sum("a", "b");
    }
}
~~~

> OBS: como é possível ver ao usar o `expetException` é necessário coloca-lo antes da chamada do método (isso se deve ao PHP ser interpretado).

<div id='mocks'></div>

## *- Entendendo Mocks:*

- Mocks são dubles das suas classes, sendo assim eles servem para fazer com que o principio dos testes unitarios continue correto em sua aplicação.

- Ao criar um mock ele herdará toda a estrutura da sua classe, porém terá alguns recursos(como método `method`) a mais e caracteristicas especificas.

- Sobre as caracteristicas especificas que posso estar citando aqui são:
    - Por padrão, todos os métodos da classe original são substituídos com uma implementação simulada que apenas retorna null.
    - Os argumentos passados para um método do dublê de teste não serão clonados.


- Agora que já compreendemos o básico, vamos criar um mock simples:

> OBS: A estrutura utilizada será a mesma do exemplo anterior.

- Arquivo src/More.php:

    ~~~php
    <?php

    namespace App;

    class More
    {
        public function moreX($n, $more)
        {
            return $n + $more;
        }
    }
    ~~~

- Arquivo src/Sum.php:

    ~~~php
    <?php
    namespace App;

    class Sum{
        public function sum(int $n, int $n2):int
        {
            try
            {
                return $n + $n2;
            }catch(\Exception $ex){
                return $ex;
            }
        }

        public function sumMoreOne(int $n, int $n2, \App\More $more):int
        {
            return $more->moreX($this->sum($n, $n2), 1);
        }
    }
    ~~~

- Arquivo tests/SumTest:

    ~~~php
    <?php

    require __DIR__."/../vendor/autoload.php";
    use App\Sum;
    use PHPUnit\Framework\TestCase;

    class SumTest extends TestCase
    {
        public function testShouldReturnSum()
        {
            $sum = new Sum();
            $result = $sum->sum(10, 50);
            
            $this->assertEquals(60, $result);
        }

        public function testSumMoreOne()
        {
            $sum = new Sum();
            $return = $sum->sum(10,30) + 1;
            $mock = $this->createMock(App\More::class);
            $mock->method("moreX")->will($this->returnValue($return));
            $result = $sum->sumMoreOne(10,30,$mock);
            $this->assertEquals(41, $result);
        }
    }
    ~~~

    - **Method:**
        - O método `method` tem a função de nomear qual método do duble você deseja alterar.

        - `will` tem a função mostrar o que será feito nesse método;

        - `$this->returnValue()` é um método que retornará um valor para o `will`.

> OBS: existem outras maneiras de fazes os métodos dos mocks retornarem valores, uma melhor explicação dele se encontra aqui: https://phpunit.readthedocs.io/pt_BR/latest/test-doubles.html.

<div id='sem-retorno'></div>

## *- O que fazer quando queremos testar um método que não tem retorno:*

- Nesses casos a solução será definida de acordo com cada problema, sendo assim não irei dar aqui uma "bala de prata", porém posso mostrar um caminho que você pode tomar como exemplo.

- Alguns recursos que o PhpUnit nos disponibiliza que serão muito interessantes são: `expects`(presente dentro dos Mocks), `never` e `exactly`.
    - `never`: confere se um método nunca foi chamado, e caso ele tenha sido da erro;

    - `exactly`: confere se um método foi chamado a quantidade de vezes que você parametrizou para ele.

- Exemplo:

    - Arquivo src/Sum:
        ~~~php
        <?php
        namespace App;

        class Sum{
            public function sum(int $n, int $n2):int
            {
                try
                {
                    return $n + $n2;
                }catch(\Exception $ex){
                    return $ex;
                }
            }

            public function sumMoreOne(int $n, int $n2, \App\More $more):int
            {
                return $more->moreX($this->sum($n, $n2), 1);
            }

            public function sumMoreTwo(int $n, int $n2, \App\More $more)
            {
                $more->moreX($more->moreX($this->sum($n, $n2), 1),1);
            }
        }
        ~~~

    - Arquivo tests/SumTest:

        ~~~php
        <?php

        require __DIR__."/../vendor/autoload.php";
        use App\Sum;
        use PHPUnit\Framework\TestCase;

        class SumTest extends TestCase
        {
            public function testShouldReturnSum()
            {
                $sum = new Sum();
                $result = $sum->sum(10, 50);
                
                $this->assertEquals(60, $result);
            }

            public function testSumMoreOne()
            {
                $sum = new Sum();
                $return = $sum->sum(10,30) + 1;
                $mock = $this->createMock(App\More::class);
                $mock->method("moreX")->will($this->returnValue($return));
                $result = $sum->sumMoreOne(10,30,$mock);
                $this->assertEquals(41, $result);
            }

            public function testSumMoreTwo()
            {
                $sum = new Sum();
                $return = $sum->sum(10,30) + 1;
                $mock = $this->createMock(App\More::class);
                $mock->expects($this->exactly(2))->method('moreX');
                $result = $sum->sumMoreTwo(10,30,$mock);
            }
        }
        ~~~
