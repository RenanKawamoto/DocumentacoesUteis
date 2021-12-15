# **Documentação básica sobre PHPUnit:**

## *Sumário:*
1.
2.
3.

## *- Instalação:*

- Pelo composer:
    
    ~~~
    composer require --dev phpunit/phpunit 
    ~~~

## *- Entendo a nomeclatura dos testes unitários:*

- Ao começar a criar os testes unitários com o PHPUnit é interessante aprender a nomeclatura padrão utilizada pela comunidade:
    - `tests`: Diretorio padrão onde se encontram os testes.
    - `nomeDaClasseTest`: A classe deve possuir o sufixo Test.
    - `testFunction`: Já os métodos deve possuir o préfixo test.

> Uma alternativa para o prefixo test*: é possível identificar os testes por anotações (/**@test*/)

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

## *- Entendo melhor sobre os asserts:* 

- Os asserts é uma comparação entre uma afirmação e o que realmente está sendo retornado, resumindo é o que você espera de uma determinada função e o que realmente ocorre (caso ambos sejam iguais o teste se da como correto).

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



