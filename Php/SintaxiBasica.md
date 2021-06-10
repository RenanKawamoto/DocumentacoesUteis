# **Documentação básica de php**

## *Sumário:*
1. [Sintaxe básica](#sintaxe)
    1. [Como abrir um código php ( \<?php?> )](#abertura)
    2. [Como criar um host local para rodar seus códigos php](#phps)
    3. [Como criar comentários](#comentarios)
    4. [Echo](#echo)
    5. [Como criar variaveis ( $ )](#variaveis)
    6. [Como printar variaveis ( echo $ )](#printandovariaveis)
    7. [Variaveis dinamicas ( $$ )](#variaveisdinamicas)
    8. [Pegar mais informações sobre uma variavel( var_dump )](#vardump)
    9. [Verificar tipo ( is_ )](#verificartipo)
    10. [Tipos de dados escalares](#tiposescalares)
    11. [Tipos de dados compostos](#dadoscompostos)
    12. [Tipos dados especiais](#especiais)
    13. [Diferença entre as aspas](#aspas)
    14. [Concatenação ( . )](#concatenacao)
    15. [Como criar funções ( function )](#funcao)
    16. [Escopo global ( global )](#global)
    17. [Outro método de utilizar o escopo global ( $_GLOBALS[] )](#outro)
    18. [Constantes ( define )](#constantes)
    19. [Array numerico](#arrayn)
        1. [Como criar um array sem elementos iniciais ( array() )](#arrays)
        2. [Criar um array com elementos ](#iniarray)
        3. [Como printar um array ( print_r )](#printr)
        4. [Alterar indices de um array ( => )](#indices)
        5. [Adicionar elementos no final do array ( push )](#add)
        6. [Adicionar elemento com indice no final do array](#addi)
        7. [Como printar um unico elemento](#printaru)
        8. [Ver quanto elemento existem dentro de um array ( count )](#count)
        9. [Percorrer um array com foreach](#arrayf)
    20. [Array associativo](#associativo)
        1. [Como pegar o indice e o conteudo através do foreach](#foreachi)
    21. [Matriz](#matriz)
    22. [Funções dos arrays](#funcoesarray)
    23. [Condicionais](#condicionais)
        1. [If / elseif / else](#if)
        2. [Operador ternario](#ternario)
        3. [Switch case](#switch)
    24. [Operadores](#opera)
        1. [Operadores aritméticos](#operadoresa)
        2. [Operadores de incremento e decremento](#operadoresid)
        3. [Operadores de atribuição](#operadoresat)
        4. [Operadores de comparação](#operadorescom)
        5. [Operadores lógicos](#operadoreslog)
    25. [Estruturas de repetição](#repeticao)
        1. [While](#while)
        2. [Do while](#dowhile)
        3. [For](#for)
        4. [Foreach](#foreach)
    26. [Funções para string](#funcoess)
    27. [Funções para numeros](#funcoesi)
    28. [Entendendo melhor como criar funções](#cfuncoes)
    29. [Variaveis superglobais ( $_ )](#superglobais)
    30. [Validações de inputs](#validatefilters)
    31. [Sanitização](#sanitizefilters)
    32. [Validação de tipo de variaveis](#filtervar)
    33. [Sessões](#sessoes)
        1. [Iniciar uma sessão ( session_start )](#isessao)
        2. [Utilizar sessão em outras páginas ( $_SESSION )](#usessoes)
        3. [Exibir o id das sessões ( session_id )](#sessoesid)
        4. [Limpar sessão ( session_unset )](#limparsessoes)
        5. [Destruir sessões ( session_destroy )](#dsessoes)
    34. [Criptografia](#criptografia)
        1. [Criptografia de mão dupla](#maodupla)
            1. [Base64](#base)
        2. [Criptografia de mão unica](#maounica)
            1. [Md5](#md5)
            2. [Sha1](#sha1)
    35. [Password Hash](#passhash)
    37. [Include / Include_Once / Require / Require_Once](#in)
        1. [Include](#include)
        2. [Require](#require)
        3. [Include e Require once](#once)
    38. [Cookie](#cookie)
    39. [Datas](#datas)
        1. [Date](#date)
        2. [Time](#time)
        3. [Mktime](#mktime)
        4. [Converter string para time](#stringtime)
    40. [Manipulação de arquivo](#arquivo)
        1. [Abrir arquivo](#abarquivo)
        2. [Inserindo dados em um arquivo](#inserindo)
        3. [Fechar um arquivo](#fechararquivo)
        4. [Retornar se um arquivo já chegou ao seu fim](#fimarquivo)
        5. [Retornar o conteúdo de cada linha do arquivo](#linhaarquivo)
        6. [Retorna quantas linhas existem dentro do arquivo](#filesize)
    41. [Expressões regulares](#expressoes)
2. [Crud com php e postgres](#crud)
    1. [Comandos](#comandosc)
        1. [Criando conexão com o banco de dados ( pg_connect )](#pgconnect)
        2. [Criando query ( pg_query )](#pgquery)
        3. [Transformando o resultado da query em uma matriz ( pg_fetch_all )](#pgfetchall)
3. [Programação orientada a objetos](#poo)
    1. [Classes](#class)
        1. [Atributos](#atributos)
        2. [Metodos](#metodos)
        3. [Criando classes](#criandoclasse)
        4. [Acessando atributos dentro da classe ($this->)](#this)
    2. [Objetos](#objetos)
        1. [Instanciando uma classe (new)](#new)
        2. [Utilizando atributos publicos (->)](#utilizandoatributospublicos)
        3. [Utilizando métodos publicos (->)](#uao)
    3. [Modificadores de acesso](#modificadoresdeacesso)
    4. [Retornando valores](#return)
    5. [Getters e Setters](#getset)
        1. [Getter](#get)
        2. [Setter](#set)
    6. [Construtor ( __construct )](#construtor)
    7. [Herança (extends)](#heranca)
    8. [Abstratação (abstract)](#abstract)
        1. [Classes abstratas](#abstractclass)
        2. [Métodos abstratos](#abstractmethod)
    9. [Atributos constantes](#const)
        1. [Como definir atributos constantes](#defineconst)
        2. [Como pegar atributos constantes (self::)](#getconst)
        3. [Como pegar constantes de uma classe herdada (parents)](#parents)
    10. [Static](#static)
        1. [Atributos estaticos](#atributesstatic)
        2. [Metodos estaticos](#methodstatic)
    11. [Polimorfismo](#polimorfismo)
    12. [Interfaces](#interfaces)
    11. [Namespace](#namespace)
        1. [Como criar um namespace](#createnamespace)
        2. [Como utilizar a " \ " e os " : "](#usetwopoints)
        3. [Como utilizar o " use "](#use)
        4. [Como apelidar o namespace](#namenamespace)
    12. [Referencia e clonagem de objetos](#referenceclone)
        1. [Clone](#clone)
        2. [Método mágico clone ( __clone )](#mclone)
    13. [Tratamento de exceções](#excecao)
        1. [Como gerar um exceção](#exception)
        2. [Como tratar exceções (try catch)](#trycatch)
        3. [Métodos das exceções](#metodosexception)
    14. [Relações entre objetos](#relacoes)
        1. [Associação](#associacao)
        2. [Agregação](#agregacao)
        3. [Composição](#composicao)
    15. [Métodos mágicos](#metodosmagicos)
4. [Como tipar o seu código php orientado a objetos](#pootipado)
    1. [Como tipar propriedades](#tipandopropriedades)
    2. [Como tipar métodos e retornos](#tipandometodoseretornos)

<div id = "sintaxe"></div>

## *Sintaxe básica:*

<div id = "abertura"></div>

### **Abertura de um código php:**

Sintaxe:
~~~
    <?php
        ...
    ?>
~~~

OBS: Essa abertura permite que você coloque códigos html misturados com os do php.

Exemplo:

~~~html
    <html>
        <head>
            <title>Titulo</title>
        </head>

        <body>
            <h1>O que vem a seguir é php</h1>
            <?php
                echo "<h2> ola mundo </h2>";
            ?>
        </body>
    </html>
~~~

<div id = "phps"></div>

### **Como criar um host local para rodar seus códigos php, sem um servidor web:**

Esse comando deve ser usado no prompt a sua escolha, e ira gerar um servidor para você fazer seus testes:

Sintaxe:
~~~
    php -S <Host>:<porta>
~~~
*Nesse caso a flag `-S` se trata de um argumento para adicionar um host local com porta definida*

Exemplo:
~~~
    php -S localhost:8000
~~~

<div id = "comentarios"></div>

### **Comentarios:**

Sintaxe comentario de um linha:
~~~php
    // aqui fica seu comentario
~~~

Sintaxe comentario em bloco:
~~~php
    /*
        esse é um
        bloco de comentario
    */
~~~

<div id = "echo"></div>

### **Mostrando elementos html com o php (echo):**

Sintaxe: 
~~~php
    <?php
        echo "<mensagem>";
    ?>
~~~

Sintaxe 2:
~~~php
    <?php
        print "<mensagem>";
    ?>
~~~

OBS: As tags html também funcionam.

<div id = "variaveis"></div>

### **Variaveis:**

Sintaxe:
~~~php
    $<nomeDaVariavel> = <valorDaVariavel>;
~~~

Exemplo:
~~~php
    $nome = "Joao";
~~~

OBS: As variaveis no php são case sensitive, e seguem as mesmas regras de identificação das outras linguagens (Não podendo haver espaço, nem iniciar com numero, ter caracteres especiais e etc).

<div id = "printandovariaveis"></div>

### **Printando variaveis:**

Sintaxe:
~~~php
    $<nomeDaVariavel> = <valorDaVariavel>;

    echo $<nomeDaVariavel>;
    echo "bla bla bla $<nomeDaVariavel>";
~~~

Exemplo:
~~~php
    $nome = "Roberto";
    echo $nome;
    echo "Ola meu nome é $nome";
~~~

<div id = "variaveisdinamicas"></div>

### **Variaveis dinamicas / Variaveis de variaveis:**

Sitaxe:
~~~php
    $<nomeDaVariavel-1> = <valorDaVariavel-1>;
    $$<nomeDaVariavel-1> = <valorDaVariavel-2>;

    echp "bla bla bla $<valorDaVariavel-1>";
~~~

Exemplo:
~~~php
    $pessoa = "nome";
    $$pessoa = "Felipe";

    echo "Ola meu nome é $nome";
~~~
*Nesse caso, a variavel nome é criada devido ao $$pessoa, que faz o processo de atribuir o conteudo de $pessoa a outra variavel*

<div id = "vardump"></div>

### **Pegar mais informações sobre uma variavel:**

Sintaxe:
~~~php
    var_dump($<variavel>);
~~~

Exemplo:
~~~php
    $nome = "Lucas";
    var_dump($nome);
~~~
OBS: O var_dump irá fazer um echo automaticamente.

<div id = "verificartipo"></div>

### **Verificar se uma variavel é de certo tipo:**

Sintaxe:
~~~php
    is_<tipo>($<variavel>);
~~~

Exemplo:
~~~php
    is_string($nome);
~~~

OBS: No caso essa é uma função que retorna um boolean (true ou false).

<div id = "tiposescalares"></div>

### **Tipos de dados escalares:**

Os tipos escalares são:
* String (que se trata de um conjunto de caracteres, que são definidos dentro das aspas duplas);
* int (trata-se de um numero inteiro);
* float (trata-se de um numero que tenha um ponto flutuante);
* boolean (trata-se de um valor true ou false);

<div id = "dadoscompostos"></div>

### **Tipos de dados compostos:**

Os tipos compostas são:
* array (conjunto de dados variaveis):

Sintaxe:
~~~php
    $<nomeDaVariavel> = array(<valor1>, <valor2>, ...);
~~~

Exemplo:
~~~php
    $numeros = array(1,2,3,4,5,6);
~~~

* object (é a instanciação de uma classe [Veremos isso mais detalhadamente a frente]);

<div id = "especiais"></div>

### **Tipos de dados especiais:**

Os tipos de dados especiais são:
* NULL (um valor vazio);
* Resource (um tipo de dado que é utilizado para fazer referencia a recursos externos [Exemplo: Manipulação com o banco de dados, manipulação de arquivos e etc])

<div id = "aspas"></div>

### **Diferença entre as aspas:**

* `'` são literais, sendo assim todo conteudo dentro dela será interpretado como uma string.

* `"` são interpretativas, sendo assim caso haja uma variavel ela irá entender e colocar seu conteudo.

<div id = "concatenacao"></div>

### **Concatenação:**

No php utilizamos o `.` para concatenar as informações.

Exemplo:
~~~
    $nome = "Maria";
    echo 'Ola meu nome é '.$nome.' e tenho 23 anos';
~~~

<div id = "funcao"></div>

### **Como criar uma função:**

Sintaxe:
~~~php
    function <nomeDaFunção>($<argumentos>){
        ...
    }
~~~

Exemplo:
~~~php
    function soma($n, $n2)
    {
        echo $n+$n2;
    }
~~~

<div id = "global"></div>

### **Escopo global:**

Para acessar variaveis que estão fora do seu escopo, podemos utilizar o `global`.

Sintaxe:
~~~php
    $<nomeDaVariavelGlobal> = <conteudoDaVariavel>;
    function <nomeDaFunção>($<argumentosDaFunção>)
    {
        global <nomeDaVariavelGlobal>
        echo $<nomeDaVariavelGlobal>;
    }
~~~

Exemplo:
~~~php
    $nome = "Julia";
    function exibeNome()
    {
        global $nome;
        echo $nome;
    }
~~~

<div id = "outro"></div>

**Outro método:**

Sintaxe:
~~~php
    $<nomeDaVariavelGlobal> = <conteudoDaVariavel>;
    function <nomeDaFunção>($<argumentosDaFunção>)
    {
        echo $GLOBALS['<nomeDaVariavelGlobal>'];
    }
~~~

Exemplo:
~~~php
    $nome = "Julia";
    function exibeNome()
    {
        echo $GLOBALS['nome'];
    }
~~~

<div id = "constantes"></div>

### **Constantes:**

No php, definimos uma constante com a função `define`.

OBS: igualmente em outras linguagens, constantes são nomeadas com todas as letras em upper case.

Sintaxe:
Sintaxe:
~~~php
    define("<nomeDaConstante>", "<valorDaConstante>");
~~~

Exemplo:
~~~php
    define("PI", 3.14);
    echo "O valor de pi = ".PI;
~~~

OBS: As `"` não conseguem interpretar constantes, assim é necessário concatenar.

OBS-2: As constantes podem ser arrays.

OBS-3: As constantes sempre são globais, assim podem ser usadas em qualquer escopo.

<div id = "arrayn"></div>

### **Array (numerico):**

<div id = "arrays"></div>

- **Como definir um array sem elementos iniciais:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray> = array();
        ~~~

    - Exemplo:
        ~~~php
            $carros = array();
        ~~~

<div id = "iniarray"></div>

- **Como definir um array com elementos iniciais:**

    - Sintaxe 1:
        ~~~php
            $<nomeDoArray> = array(<valor1>, <valor2>, ...);
        ~~~

    - Sintaxe 2:
        ~~~php
            $<nomeDoArray> = [<valor1>, <valor2>, ...];
        ~~~

    - Exemplo:
        ~~~php
            $carros = array("gol", "fusca", "bmw");
        ~~~

        ~~~php
            $carros = ["gol", "fusca", "bmw"];
        ~~~

<div id = "printr"></div>

- **Como printar um array:**

    - Sintaxe:
        ~~~php
            print_r($<nomeDaVariavelDoArray>)
        ~~~

    - Exemplo:
        ~~~php
            print_r($carros);
        ~~~

<div id = "indices"></div>

- **Como alterar os indices de um array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray> = array(<n°DoIndice> => <valor1>, <valor2>, ...);
        ~~~

    - Exemplo:
        ~~~php
            $carros = array(1 => "gol", "fusca", "bmw");
        ~~~

<div id = "add"></div>

- **Como adicionar elementos no final do array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray>[] = <valorASerAdicionado>
        ~~~
    
    - Exemplo:
        ~~~php
            $nomes[] = "Luiz";
        ~~~

<div id = "addi"></div>

- **Como adicionar elementos com um indice no final do array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray>[<indice>] = <valorASerAdicionado>
        ~~~
    
    - Exemplo:
        ~~~php
            $nomes[5] = "Luiz";
        ~~~

<div id = "printaru"></div>

- **Como printar um unico elemento do array:**

    - Sintaxe:
        ~~~php
            echo $<nomeDoArray>[<indice>];
        ~~~

    - Exemplo:
        ~~~php
            echo $nomes[0];
        ~~~

<div id = "count"></div>

- **Como ver quantos elementos existem dentro do array:**

    - Sintaxe:
        ~~~php
            count($<nomeDoArray>);
        ~~~

    - Exemplo:
        ~~~php
            echo count($nomes)
        ~~~

<div id = "arrayf"></div>

- **Como percorrer um array com foreach:**

    - Sintaxe:
        ~~~php
            foreach($<nomeDoArray> as $<nomeDaVariavelLocal>)
            {
                ...
            }
        ~~~

        *Nesse caso a variavel local será igual a um elemento do array por loop*

    - Exemplo:
        ~~~php
            foreach($pessoas as $pessoa)
            {
                echo var_dump(pessoa);
            }
        ~~~

<div id = "associativo"></div>

### **Array associativo:**

- São iguais aos numericos, porém com a diferença que definimos os indices para serem strings.

   - Sintaxe:
        ~~~php
            $<nomeDoArray> = array(<chaveIndice> => <valor1>, <chaveIndice2> => <valor2>, ...);
        ~~~

    - Exemplo:

        ~~~php
            $pessoa = array("nome" => "Luiza", "idade" => 35);
        ~~~

<div id = "foreachi"></div>

- **Como pegar o indice e o valor no foreach:**

    - Sintaxe:
        ~~~php
            foreach($<nomeDoArray> as $<indice> => $<valor>)
            {
                ...
            }
        ~~~

    - Exemplo:
        ~~~php
            foreach($nomes as $indice => $conteudo)
            {
                echo $indice.': '.$conteudo;
            }
        ~~~

<div id = "matriz"></div>

### **Arrays multidimensionais:**

É quando colocamos um array dentro de outro.

Exemplo:
~~~php
    $pessoas = array(
        "Joao" => array(17, "paulista"),
        "Maria" => array(24, "mineira")
    )
~~~
OBS: para chamar esses elementos é necessário um indice duplo, exemplo: $pessoa["Maria"][0].

<div id = "funcoesarray"></div>

### **Funções de arrays:**

* `is_array($array)` ==> verifica se o elemento entre os () é um array, retornando um boolean;

* `in_array($valor, $array)` ==> verifica se o valor passado está dentro do array, retornando um boolean.

* `array_keys($array)` ==> retorna um novo array com as chaves do array passado.

* `array_values($array)` ==> retorna um novo array com os valores do array passado.

* `array_merge($array1, $array2)` ==> agrega o conteúdo dos arrays.

* `array_pop($array)` ==> exclui o ultimo elemento do array, e retorna qual o valor que foi excluido.

* `array_unshift($array, "valor1", "valor2", ...)`==> adiciona um ou mais elementos no inicio do array.

* `array_push($array, "valor1", "valor2", ...)`==> adiciona um ou mais elementos no final do array.

* `array_combine($keys, $values)`==> mescla os dois arrays passados, como chave-valor.

* `array_sum($array)`==> calcula a soma dos elementos de um array.

* `array_explode("divisor", $variavel)`==> transforma uma string em um array.

    - Exemplo:

        ~~~php
            $data = "30/01/2001"
            $novoArray = array_explode('/', $data);
        ~~~

* `array_implode("divisor", $array)`==> transforma um array em uma string.

    - Exemplo:

        ~~~php
            $nomes = array("Rodrigo", "Maria", "Julia", "Luiz");
            $palavra = implode ("-", $nomes);

            echo $palavra; // irá imprimir Rodrigo-Maria-Julia-Luiz 
        ~~~

<div id = "condicionais"></div>

### **Condicionais:**

<div id = "if"></div>

- **If/Elseif/Else:**

    - Sintaxe:
        ~~~php
            if (<condição>):
                ...
            elseif (<condição2>):
                ...
            else:
                ...
            endif;
        ~~~
    
    - Exemplo:
        ~~~php
            $n = 45;
            if ($n < 20):
                echo "n eh menor que 20";
            elseif ($n == 40):
                echo "n eh igual a 40";
            else:
                echo "n eh maior que 20";
            endif;
        ~~~

<div id = "ternario"></div>

- **Operador ternario:**

    - Sintaxe:
        ~~~php
            echo (<condicional>) ? <codigoSeForTrue> : <codigoSeForFalse>;
        ~~~

    - Exemplo:
        ~~~php
            $media = 6;
            echo ($media >= 7) ? "Aprovado" : "Reprovado";
        ~~~

<div id = "switch"></div>

- **Switch case:**

    - Sintaxe:
        ~~~php
            switch($<variavelQueDesejaComparar>):
                case <ValorComparado>:
                    ...
                    break;
                default:
                    ...
            endswitch;
        ~~~
        OBS: O switch case sempre será um comparação de igualdade.

    - Exemplo:
        ~~~php
            switch($media):
                case 10:
                    echo "Nota 10";
                    break;
                case 9:
                    echo "Nota 9";
                    break;
                default:
                    echo "Nota menor que 9";
            endswitch;
        ~~~

<div id = "opera"></div>

### **Operadores:**

<div id = "operadoresa"></div>

- **Operadores aritméticos:**


    * `Adição( + )`
    * `Subtração( - )`
    * `Multiplicação ( * )`
    * `Divisão ( / )`
    * `Módulo ( % )`
    * `Exponenciação ( ** )`

<div id = "operadoresid"></div>

- **Operadores de Incremento e Decremento:**

    * Pré-Incremento e pré-decremento:
        - Tem a função de incrementar o valor e depois utiliza-lo.

            Sintaxe:
            ~~~php
                ++$<variavelOuNumero>;
                --$<variavelOuNumero>;
            ~~~

    * Pós-Incremento e pós-decremento:
        - Tem a função de incrementar o valor apenas após utiliza-lo.

            Sintaxe:
            ~~~php
                $<variavelOuNumero>++;
                $<variavelOuNumero>--;
            ~~~

        Exemplos:
        ~~~php
            $n = 3;

            echo ++$n; // irá printar 4;

            $n = 3;

            echo $n++ // irá printar 3, porém o valor será alterado após o comando;
        ~~~

<div id = "operadoresat"></div>

- **Operadores de atribuição:**

    * `=` ---> é um operador de atribuição simples, sendo assim ele irá transferir o valor da sua direta para a variavel na sua esquerda.
    * `-=` ---> é um operador de atribuição que irá decrementar o valor que está na esquerda ao valor da direita.
    * `+=` ---> é um operador de atribuição que irá incrementar o valor que está na esquerda ao valor da direita.
    * `*=` ---> é um operador de atribuição que irá multiplicar o valor que está na esquerda ao valor da direita.
    * `/=` ---> é um operador de atribuição que irá dividir o valor que está na esquerda ao valor da direita.
    * `%=` ---> é um operador de atribuição que irá pegar o resto da divisão do valor que está na esquerda ao valor da direita.

<div id = "operadorescom"></div>

- **Operadores de comparação:**
    * `==` ---> compara o conteudo, vendo se são iguais.
    * `!=` ---> compara se o conteudo, vendo se são diferentes.
    * `===` ---> compara se um valor é identico ao outro (vendo até os tipos).
    * `!==` ---> compara se um valor não é identico ao outro.
    * `<>` ---> ve se os valores são diferentes.
    * `<` ---> compara se o valor é menor que o outro.
    * `>` ---> compara se o valor é maior que o outro.
    * `<=` ---> compara se o valor é menor ou igual ao outro.
    * `>=` ---> compara se o valor é maior ou igual ao outro.
    * `<=>` ---> compara os dois valores e retorna: se o da esquerda for menor -1 / se o da direita for menor 1 / se os dois forem iguais 0.

<div id = "operadoreslog"></div>

- **Operadores lógicos:**
    * `&&` ou `and` ---> retorna true se os dois lados derem true;
    * `||` ou `or` ---> retorna true se um dos lados ou os dois derem true;
    * `xor` ---> retorna true quando apenas uma das condições for verdadeira;
    * `!` ---> inverte o estado lógico da expressão.

<div id = "repeticao"></div>

### **Estruturas de repetição:**

<div id = "while"></div>

- **While:**
    - Sintaxe:
        ~~~php
            while (<condição>):
                ...
            endwhile;
        ~~~

    - Exemplo:
        ~~~php
            $contador = 1;
            while($contador < 10):
                echo $contador;
                $contador++;
            endwhile;
        ~~~

<div id = "dowhile"></div>

- **Do while:**
    - Tem a mesma função do while, porém com o `Do` o loop será executado pelo menos uma vez.

    - Sintaxe:
        ~~~php
            do
            {
                ...
            }while(<condição>);
        ~~~

    - Exemplo:
        ~~~php
            $contador = 1;
            do
            {
                echo $contador;
                $contador++;
            }while($contador < 10);
        ~~~

<div id = "for"></div>

- **For:**
    - Sintaxe:
        ~~~php
            for (<contador>; <condição>; <incremento>):
                ...
            endfor;
        ~~~

    - Exemplo:
        ~~~php
            for ($cont = 0; $cont < 10; cont++):
                echo $cont."<br>";
            endfor;
        ~~~

<div id = "foreach"></div>

- **Foreach:**
    - Tem a função de percorrer um array atribuindo cada desse em uma variavel local.

    - Sintaxe:
        ~~~php
            foreach ($<array> as $<variavel>):
                ...
            endforeach;
        ~~~

    - Exemplo:
        ~~~php
            $pessoas = ["Lucas","Luna","Mario"];
            foreach ($pessos as $pessoa):
                echo $pessoa."<br>";
            endforeach;
        ~~~

    OBS: É possível pegar os indices do array utilizado mais uma variavel no foreach e `=>`:

    - Exemplo:
        ~~~php
            $pessoas = ["Lucas","Luna","Mario"];
            foreach ($pessos as $indice => $pessoa):
                echo $indice.": ".$pessoa."<br>";
            endforeach;
        ~~~

<div id = "funcoess"></div>

### **Funções para string:**

* `strtoupper($string)` ===> passa a string para maiuscula;
* `strtolowe($string)` ===> passa a string para minuscula;
* `substr($string, CaractereQueDesejaIniciar, QuantosCaracteresIraAndar)` ===> irá retorna uma parte da string:
    - Exemplo:
        ~~~php
            $mensagem = "ola mundo";
            echo substr($mensagem, 4, 4); //ira printar mund
        ~~~

* `str_pad($string, QuantidadeDeCaracteresDesejada, "ElementoQueIraPrencherOsNovosCaracteres", EsseCampoÉOpcionalParaColocarOsNovosCaracteresAEsquerda<STR_PAD_LEFT>)` ===> irá acrescentar "n" caracteres até o comprimento desejado;
    - Exemplo:
        ~~~php
            $mensagem = "ola";
            echo std_pad($mensagem, 5, "*"); //irá printar ola**
        ~~~
    OBS: É possível coloca os caracteres a direita com STR_PAD_RIGHT / a esquerda com STR_PAD_LEFT / distribuir STR_PAD_BOTH;

* `str_repeat(StringQueQuerRepetir, numeroDeVezes)` ===> irá retornar uma string repetidas "n" vezes;

* `strlen($string)` ===> retorna o comprimento da string;

* `str_replace(PalavraQueSeraSubstituida, PalavraQueVaiSubstituir, $string)` ===> irá substituir a palavra na sua string;

* `strpos($string, palavraQueQuerRetornarAPosição)` ===> retorna a posição de uma palavra em uma string.

<div id = "funcoesi"></div>

### **Funções para números:**

* `number_format($numero, casasDecimais, separadorDecimal, separadorDeMilhares)` ===> irá formatar os numeros floats:

    - Exemplo:
    ~~~php
        $numero = 1425.255555;
        echo number_format($numero, 2, ",", "."); // ira printar 1.425,26
    ~~~

* `round(valor)` ===> serve para arredondar valores;

* `ceil(valor)` ===> serve para arredondar valores, porém sempre para cima;

* `floor(valor)` ===> serve para arredondar valores, porém sempre para baixo;


* `rand(valorInicial, valorFinal)` ===> irá retornar um valor aleatorio entre intervalo numerico;

<div id = "cfuncoes"></div>

### **Mais de como criar funções:**

Sintaxe:
~~~php
    function <nomeDaFunção>($<parametros>)
    {
        ...
        <opcional> return ...;
    }
~~~

Exemplo1:
~~~php
    function media($n1, $n2)
    {
        if(($n1+$n2)/2 < 7):
            echo "Reprovado";
        else:
            echo "Aprovado";
        endif;
    }

    media(10, 9); //irá printar aprovado
~~~

Exemplo2:
~~~php
    function media($n1, $n2)
    {
        if(($n1+$n2)/2 < 7):
            return "Reprovado";
        else:
            return "Aprovado";
        endif;
    }

    echo media(1, 2); //irá printar reprovado
~~~

<div id = "superglobais"></div>

### **Variaveis superglobais:**

- Essas variaveis sempre estão disponiveis, independente do escopo.

* `$GLOBALS['nomeDaVariavel']` ===> tem a função de disponibilizar as variaveis globais para uso.
   
    - Exemplo:
        ~~~php
            $numero = 10;
            function MaisUm()
            {
                echo ++$GLOBALS['numero'];
            }
        ~~~

* `$_SERVER['Indice']` ===> contem varias iformações sobre o server, que podem ser acessadas através dos indices, pré estabelecidos exemplo: $_SERVER['SERVER_NAME'];

* `$_POST['nomeDoInput']` ===> tem a função de pegar os dados contidos em um formulario POST:

    - Sintaxe Formulario POST:
        ~~~php
            <html>
                <body>
                    <form action = "path/arquivo.php" method="POST">
                        BlaBla <input type = "<tipo>" name="<nomeQueSeráPassadoNo$_POST[]>">
                        <button type = "submit">BlaBla</button>
                    </form>
                </body>
            </html>
        ~~~

    - Sintaxe Arquivo para pegar o POST:
        ~~~php
            <?php
                $<nomeDaVariavel> = $_POST['<nomeDoInputDoForms'];
            ?>
        ~~~

    - Exemplo Formulario POST:
        ~~~php
            <html>
                <body>
                    <form action = "dados/dado.php" method="POST">
                        Nome <input type = "text" name="name">
                        <button type = "submit">Enviar</button>
                    </form>
                </body>
            </html>
        ~~~

    - Exemplo arquivo para pegar o POST:
        ~~~php
            <?php
                $nome = $_POST['name'];
                echo $nome;
            ?>
        ~~~

* `$_GET['nomeDaVariavel']` ===> tem a função de pegar os dados contidos na URL:


    - Sintaxe Formulario GET:
        ~~~php
            <html>
                <body>
                    <form action = "path/arquivo.php" method="GET">
                        BlaBla <input type = "<tipo>" name="<nomeQueSeráPassadoNo$_GET[]>">
                        <button type = "submit">BlaBla</button>
                    </form>
                </body>
            </html>
        ~~~

    - Sintaxe Arquivo para pegar o GET:
        ~~~php
            <?php
                $<nomeDaVariavel> = $_GET['<nomeDoInputDoForms'];
            ?>
        ~~~

    - Exemplo Formulario GET:
        ~~~php
            <html>
                <body>
                    <form action = "dados/dado.php" method="GET">
                        Nome <input type = "text" name="name">
                        <button type = "submit">Enviar</button>
                    </form>
                </body>
            </html>
        ~~~

    - Exemplo arquivo para pegar o GET:
        ~~~php
            <?php
                $nome = $_GET['name'];
                echo $nome;
            ?>
        ~~~

    OBS: como essas informações vem da URL não há necessidade de passarmos um formulario:

    Exemplo arquivo para enviar o GET:
    ~~~php
        <a href="/dados/dados.php?name=Felipe&age=48">Mandar</a>
    ~~~
    *Nesse caso utilizamos o "?" para mostrar ao programa que tudo que vier após isso na URL é um parametro, sendo assim o parametro name contem o conteudo Felipe (o & em questão adiciona mais um parametro) e o age contem 48.*

<div id = "validatefilters"></div>

### **Validações(Validate Filters):**

* `isset($_POST['<indice>'])` ===> retorna um boolean se existir o indice;

* `filter_input(<verbo_do_input>, <nome_do_input>, <tipo_do_filtro>)` ===> irá validar o input segundo algum filtro, retornando true ou false:

    - Exemplo:
    ~~~php
        if(filter_input(INPUT_POST, 'idade', FILTER_VALIDATE_INT))
        {
            echo 'O valor é um inteiro';
        }
    ~~~

    - `**Alguns Validate Filters:**`
        - `FILTER_VALIDATE_INT` ===> valida se o input é um inteiro;
        - `FILTER_VALIDATE_EMAIL` ===> valida se o input tem as caracteristicas de um email;
        - `FILTER_VALIDATE_FLOAT`  ===> valida se o input é do tipo float;
        - `FILTER_VALIDATE_IP`  ===> valida se o input segue um padrão de ip;
        - `FILTER_VALIDATE_URL`  ===> valida se o input segue as caracteristicas de uma url;

<div id = "sanitizefilters"></div>

### **Sanitização(Sanitize Filters):**

- Esses filtros tem a intenção de "limpar" os inputs para que fiquem no formato solicitado.

- `filter_input(<verbo_do_input>, <nome_do_input>, <filter_sanitize>)`, esse filtro irá entender somente o que é correto para seu tipo:

    - Exemplo: 
    ~~~php
        filter_input(INPUT_POST, 'nome', FILTER_SANITIZE_SPECIAL_CHARS);    
    ~~~

    - `**Alguns Sanitize Filters:**`
        - `FILTER_SANITIZE_SPECIAL_CHARS` ===> irá escapar todas as informações;
        
        - `FILTER_SANITIZE_NUMBER_INT` ===> irá pegar somente os numeros de um input.

        - `FILTER_SANITIZE_EMAIL` ===> irá pegar apenas os caracteres que podem compor um email, dentro do input.

        - `FILTER_SANITIZE_URL` ===> irá pegar apenas os caracteres que podem compor uma URL, dentro do input.

<div id = "filtervar"></div>

### **Validando variaveis(filter_var):** 
Essa validação irá ver se uma variavel passa pelos padrões de um determinado filtro, retornando um boolean.

Sitaxe:
~~~php
    filter_var($<variavel>, <filtroDesejado>);
~~~

Exemplo:
~~~php
    $n = 12;
    if(filter_var($n, FILTER_VALIDATE_INT))
    {
        echo "O numero é inteiro";
    }
~~~

<div id = "sessoes"></div>

### **Sessões:**

- Tem a função de armazenar informações, que poderão ser acessadas em diversas páginas, através da superglobal $_SESSION.

    <div id = "isessao"></div>

    - **Iniciar uma sessão:**
        - Sintaxe:
            ~~~php
                session_start();

                $_SESSION['NomeDaSessão'] = "valor contido na sessão"; //Isso irá criar as sessões
            ~~~

        - Exemplo:
            ~~~php
                session_start();

                $_SESSION['cor'] = "vermelho";
            ~~~

    <div id = "usessoes"></div>

    - **Utilizar em outras páginas:**
        - Exemplo:
            ~~~php
                session_start();

                echo $_SESSION['cor']; //irá printar vermelho;
            ~~~

    <div id = "sessoesid"></div>

    - **Exibir o id da sessão:**
        - Sintaxe:
            ~~~php
                session_start();
                session_id();
            ~~~

    <div id = "limparsessoes"></div>

    - **Limpar a sessão:**
        - Sintaxe:
            ~~~php
                session_unset();
            ~~~

    <div id = "dsessoes"></div>

    - **Destruir a sessão:**
        - Sintaxe:
            ~~~php
                session_destroy();
            ~~~

<div id = "criptografia"></div>

### **Criptografia:**

<div id = "maodupla"></div>

- **Criptografia de mão dupla:**
    - Essas criptografias podem ser criptografas e descriptografas com suas respectivas funções.

    <div id = "base"></div>

    - **Base64:**

        - Sintaxe:
        ~~~php
            $<variavel> = base64_encode($<variavelSenha>);

            $<variavelDescripto> = base64_decode($<variavel>);
        ~~~

        - Exemplo:
        ~~~php
            $senha = "123456"
            $senhaCripto = base64_encode($senha);

            $senhaDescripto = base64_decode($senhaCripto);
        ~~~

<div id = "maounica"></div>

- **Criptografia de mão unica:**
    - Essas criptografias podem ser somente criptografas.

    <div id = "md5"></div>

    - **Md5:**

        - Sintaxe:
        ~~~php
          $<variavel> = md5($<variavelSenha>);
        ~~~ 

        - Exemplo:
        ~~~php
            $senha = "123456";
            $senhaCripto = md5($senha);
        ~~~ 

    <div id = "sha1"></div>

    - **Sha1:**

        - Sintaxe:
        ~~~php
          $<variavel> = sha1($<variavelSenha>);
        ~~~ 

        - Exemplo:
        ~~~php
            $senha = "123456";
            $senhaCripto = sha1($senha);
        ~~~ 

<div id = "passhash"></div>

### **Password_hash:**

É uma maneira que foi implementada pelo php para gerar uma criptografia mais segura para suas senhas.

Sintaxe Criptografar:
~~~php
    $<variavel> = password_hash($<senha>, <AlgoritmoQueIraGerarASenha>, <ArrayDeOpçõesOpcionalQueSoApresentaAChaveCost>);
~~~

Exemplo:
~~~php
    $options = ['cost' => 10]; // quanto maior o cost, mais segura será a cripto, porém irá demorar mais para gerar
    $senha = "123456";
    $senhaCripto = password_hash($senha, PASSWORD_DEFAULT, $options); //lembre-se que o $options é um argumento opcional, o default dele é 10
~~~

Sintaxe Altenticar (ira retornar um boolean):
~~~php
    $<variavel>;
    $<variavelCripto>;
    if(password_verify($<variavel>, $<variavelCripto>))
    {
        ...
    }
~~~

Exemplo:
~~~php
    $senha = "123456";
    $senhaCripto = password_hash($senha, PASSWORD_DEFAULT);
    if(password_verify($senha, $senhaCripto)
    {
        echo "Senha correta";
    }
    else
    {
        echo "Senha incorreta";
    }
~~~

<div id = "in"></div>

### **Include / Include_Once / Require / Require_Once:**

- Tanto o include, quanto o require tem a função de incluir códigos php em outros arquivos .php, porém existe uma pequena diferença.

<div id = "include"></div>

- **Include:**
    - O include da um warning caso o arquivos que você deseja adicionar não exista.

    - Sintaxe:
        ~~~php
            include '<nomeDoArquivo.php>';
        ~~~

    - Exemplo:
        ~~~php
            include 'ola.php';
        ~~~

<div id = "require"></div>

- **Require:**
    - O require para o script caso o arquivo que você deseja adicionar não exista.

    - Sintaxe:
        ~~~php
            require '<nomeDoArquivo.php>';
        ~~~

    - Exemplo:
        ~~~php
            require 'ola.php';
        ~~~

<div id = "once"></div>

- **Include e Require once:**

    - Tem a mesma função que os seus respectivos comando sem o once, porém verificam se o código já foi adicionado, e se já tiver sido, não adiciona novamente.

<div id = "cookie"></div>

### **Cookies:**

Cookie é um arquivo criado pelo servidor no computador do usuario para armazenar algumas informações.

Sintaxe setar um cookie:
~~~php
    setcookie('<nome>', '<valorDoCookie>', time()+<quantidadeEmSegundos>);
~~~

Exemplo:
~~~php
    setcookie('user', 'teste', time()+3600); //um cookie user com conteudo teste e que dura 1h
~~~

Exemplo acessar info:
~~~php
    echo $_COOKIE['name'];
~~~

<div id = "datas"></div>

### **Datas:**

<div id = "date"></div>

- **Date():**
    - Todas as instruções a seguir são utilizadas dentro da função date();

        * `d` ===> retorna o dia atual no formato de dois digitos;
        * `D` ===> retorna o dia da semana na forma de dois caracteres;
        * `m` ===> retorna o mês atual no formato de dois digitos;
        * `M` ===> retorna o mês atual em formato textual com três caracteres;
        * `y` ===> retorna o ano em dois digitos;
        * `Y` ===> retorna o ano em formato de quatro digitos;
        * `l` ===> retorna o dia da semana; 
        * `h` ===> retorna a hora no formato de 12 horas;
        * `H` ===> retorna a hora no formato de 24 horas;
        * `i` ===> retorna os minutos;
        * `s` ===> retorna os segundos;
        * `A` ===> retorna o PM ou AM;

    - Como definir a timezone:

        - Exemplo:
            ~~~php
                date_default_timezone_set('America/Sao_Paulo'); 
            ~~~ 

<div id = "time"></div>

- **Time():**
    - Ele retorna o tempo em segundos desde 1970, até a data atual.

    - Caso queira formatar, basta utilizar a seguinte sintaxe:

        Exemplo:
        ~~~php
            date('d/m/y', time());
        ~~~

<div id = "mktime"></div>

- **Mktime():**
    - É uma outra maneira de criar uma data, com horas, minutos e segundos.

    - Sintaxe:
        ~~~php
            $data_pagamento = mktime(<hora_com_dois_digitos>, <minutos_com_dois_digitos>, <segundos_com_dois_digitos>, <mes_com_dois_digitos> , <dia_com_dois_digitos>, <ano_com_quatro_digitos>)
        ~~~

    - Exemplo:
        ~~~php
            $data_pagamento = mktime(20, 25, 10, 12, 01, 2001);
        ~~~

    - Como formatar:
        ~~~php
            $data_pagamento = mktime(20, 25, 10, 12, 01, 2001);
            date('d/m - h:i', $data_pagamento);
        ~~~

<div id = "stringtime"></div>

- **Converter string para time, e assim poder formatar:**

Para isso utilizamos a função strtotime($<variavel>);

Exemplo:
~~~php
    $data = '2020-04-10 13:20:10';
    $data = strtotime($data);
~~~

<div id = "arquivo"></div>

### **Manipulação de arquivos:**

<div id = "abarquivo"></div>

- **Abrir um arquivo:**

    - Sintaxe:
        ~~~php
            $<variavelQueFicaraOArquivo> = fopen($<nomeEExtensaoDoArquivo>, '<mode>');
        ~~~

    - Exemplo:
        ~~~php
            $arquivoAberto = fopen('olamundo.txt', 'a')
        ~~~

    - Modos:
        - `'r'` --> abre somente para leitura / coloca o ponteiro no começo do arquivo.
        - `'r+'` --> abre para leitura e escrita / coloca o ponteiro no começo do arquivo.
        - `'w'` --> abre somente para escrita / coloca o ponteiro no começo do arquivo e reduz o comprimento do arquivo para zero / se o arquivo não existir, tenta criá-lo.
        - `'w+'` --> abre para a leitura e escrita / coloca o ponteiro no começo do arquivo e reduz o comprimento do arquivo para zero / se o arquivo não existir tenta cria-lo.
        - `'a'` --> Abre somente para escrita / coloca o ponteiro do arquivo no final do arquivo / se o arquivo não existir, tenta criá-lo.
        - `'a+'` --> Abre para leitura e escrita / coloca o ponteiro do arquivo no final do arquivo / se o arquivo não existir, tenta criá-lo.
        - `'x'` --> Cria e abre o arquivo somente para escrita / coloca o ponteiro no começo do arquivo / Se o arquivo já existir, a chamada a fopen() falhará, retornando false e gerando um erro de nível E_WARNING / se o arquivo não existir, tenta criá-lo. 
        - `'x+'` --> Cria e abre o arquivo para leitura e escrita; coloca o ponteiro no começo do arquivo. Se o arquivo já existir, a chamada a fopen() falhará, retornando false e gerando um erro de nível E_WARNING / se o arquivo não existir, tenta criá-lo.        

<div id = "inserindo"></div>

- **Inserindo conteudo dentro de um arquivo:**

    - Sintaxe:
        ~~~php
            fwrite($<variavelQueContemOArquivo>, '<conteudo>');
        ~~~

    - Exemplo:
        ~~~php
            fwrite($arquivoAberto, 'ola mundo\n');
        ~~~

<div id = "fechararquivo"></div>

- **Fechar um arquivo:**

    - Sintaxe:
        ~~~php
            fclose($<varivelQueContemOArquivo>);
        ~~~

    - Exemplo:
        ~~~php
            fclose($arquivoAberto);
        ~~~

<div id = "fimarquivo"></div>

- **Retornar se o arquivo já chegou ao seu fim:**

    - Sintaxe:
        ~~~php
            feof($<variavelQueCOntemOArquivo>);
        ~~~

    - Exemplo:
        ~~~php
            if(feof($arquivoAberto))
            {
                echo "final do arquivo";
            }
        ~~~

<div id = "linhaarquivo"></div>

- **Retornar o conteudo de cada linha do arquivo:**

    - Sintaxe:
        ~~~php
            $<conteudoDaLinha> = fgets($<variavelDoArquivo>, $<tamanhoLimiteParaAParadaDaLeitura>);
        ~~~

    - Exemplo:
        ~~~php
            while(!feof($arquivoAberto))
            {
                $linha = fgets($arquivoAberto, 3);

                echo $linha.'<br';
            }
        ~~~

<div id = "filesize"></div>

- **Retornar quantas linhas existem no arquivo:**


    - Sintaxe:
        ~~~php
            $<variavel> = filesize($<variavelDoArquivo>);
        ~~~

    - Exemplo:
       ~~~php
            $tamahoArquivo = filesize($arquivoAberto);
        ~~~

<div id = "expressoes"></div>

### **Expressões regulares:**

- **preg_match():**
    - O preg_match tem a função de validar a string com base na expressão regular passada / retorna um boolean.

    - Sintaxe:
        ~~~php
            preg_match(<expressãoRegular>, $<string>);
        ~~~
    
    - Exemplo:
        ~~~php
            preg_match("/^a/", "abc");
        ~~~

- **Entendendo os comandos da expressão regular:**

    - OBS: Todos os comandos devem ser uma string entre barras.

    - Exemplo: "/^a/";

    - `^` --> indica o inicio da expressão / tudo que vier após ele tem que estar no inicio da string;

    - `$` --> indica o fim da expressão / tudo que vier antes dele deve estar no final da string;

    - `[a-z]` --> cria um conjunto de caracteres que pode ir de a até o z minuscula;

    - `[A-Z]` --> cria um conjunto de caracteres que pode ir de a até o z maiuscula;

    - `[a-zA-Z0-9]` --> cria um conjunto de caracteres que pode ir de a até o z minuscula ou maiuscul e de 0 a 9;

    - OBS: caso não deseje ficar utilizando [a-zA-Z], basta colocar um `i` após a ultima barra;

    - `[]{n,m}` --> após um conjunto, é possível adicionar chaves contendo a quantia aceita, exemplo: [a-z]{1,4} / assim será aceito palavras que contenham 1 a 4 letras minusculas;

    - `[]?` --> é um intervalo pré estabelecido que vai de 0 a 1;

    - `[]*` --> é um intervalo pré estabelecido que vai de 0 a n;

    - `[]+` --> é um intervalo pré estabelecido que vai de 1 a n;

    - OBS-2: É possível colocar dentro de um conjunto mais elementos exemplo [a-z-\_\@], assim será aceito o '-' o '_' e o '@', porém é necessário colocar \;

    - `(valor1|valor2|valor3)` --> é um maneira de criar um conjunto de palavras que serão aceitas.

<div id = "crud"></div>

## *Crud básico com o php e o postgresql:*

Antes de tudo é necessário instalar php-pgsql.

<div id = "comandosc"></div>

### **Comandos:**

<div id = "pgconnect"></div>

- `pg_connect():`
    - Tem a função de criar uma conexão com o banco, é aqui que você irá passar a porta, o user, a senha, qual é o banco e a senha.

    - Sintaxe:
        ~~~php
            $<variavel> = "host={...} port={...} dbname={...} user={...} password={...}";
            $<var> = pg_connect($<string>);
        ~~~

    - Exemplo:
        ~~~php
            $host = "localhost";
            $port = "5432";
            $dbname = "teste";
            $user = "postgres";
            $password = "password"; 
            $connection_string = "host={$host} port={$port} dbname={$dbname} user={$user} password={$password} ";
            $db = pg_connect($connection_string);     
        ~~~

<div id = "pgquery"></div>

- `pg_query():`
    - Tem a função de utilizar as queries que são passadas na conexão. (existe um retorno boolean)

    - Sintaxe:
        ~~~php
            $<variavel> = pg_query($<conexãoComObanco>, $<comandoSQL>)
        ~~~

    - Exemplo:
        ~~~php
            $consulta = pg_query($db , "select * from teste");
        ~~~

<div id = "pgfetchall"></div>

- `pg_fetch_all():`
    - Tem a função de passar o resultado da consulta para um array de arrays.

    - Sintaxe:
        ~~~php
            $<variavel> = pg_fetch_all($<variavelResultanteDaConsulta>);
        ~~~

    - Exemplo:
        ~~~php
            $consulta = pg_query($db , "select * from teste");

            $resultado = pg_fetch_all($consulta);

            print_r($resultado);
        ~~~

<div id = "poo"></div>

## *PHP Orientado a Objetos(POO):*

<div id = "class"></div>

### **Classes:**

Para entendermos classes é necessário saber o conceito geral de atributos e métodos:

<div id = "atributos"></div>

* Atributos são caracteristicas de uma classe: 

    - Exemplo:
        ~~~php
            class Carro
            {
                public $cor;
            }
        ~~~
    
        *Nesse caso temos uma classe carro com atributo $cor*

        *Peço que por hora não se importe com o termo `public`, pois mais a frente haverá um explicação completa*

<div id = "metodos"></div>

* Métodos são "ações" feitas pela classe:

    - Exemplo:
        ~~~php
            class Carro
            {
                public function buzinar()
                {
                    echo "peeeeh";
                }
            }
        ~~~
        
        *Nesse caso o método é buzinar tem a função de imprimir a onomatopeia da buzina, quando é chamada*

<div id = "criandoclasse"></div>

* Como é criado uma classe:

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                <nivelDeVisibilidade> $<atributos>;
                ...
                <nivelDeVisibilidade> function <nomeDoMétodo>($<argumentos>)
                {
                    ...;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Carro
            {
                public $cor;
                public $quantidadeDeRodas;
                public $nome;
                public function Buzinar()
                {
                    echo "peeeeeh";
                }
            }
        ~~~

<div id = "this"></div>

* Como acessar atributos dentro da classe($this->):

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                <nivelDeVisibilidade> $<atributos>;
                ...
                <nivelDeAcesso> function <nomeDoMétodo>($<argumentos>)
                {
                    echo $this-><nomeDoAtributo>;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Pessoa
            {
                public $nome;
                public function printarNome()
                {
                    echo $this->nome;
                }
            }
        ~~~

<div id = "objetos"></div>

### **Objetos:**

Um objeto é uma instancia de uma classe.

<div id = "new"></div>

* Como instanciar uma classe (new):

    - Sintaxe:
        ~~~php
            $<variavel> = new <nomeDaClasse>();
        ~~~
    
    - Exemplo:
        ~~~php
            $Julia = new Pessoa();
        ~~~

<div id = "utilizandoatributospublicos"></div>

* Como pegar/utilizar atributos publicos de um objeto (->):

    - Sintaxe:
        ~~~php
            $<variavel> = new <nomeDaClasse>();
            $<variavel>-><atributo>;
        ~~~

    - Exemplo:
        ~~~php
            $Luiz = new Pessoa();
            $Luiz->nome = "Luiz";
        ~~~

<div id = "uao"></div>

* Como utilizar métodos de um objeto (->):

    - Sintaxe:
        ~~~php  
            $<variavel> = new <nomeDaClasse>();
            $<variavel>-><método>();
        ~~~

    - Exemplo:
        ~~~php
            $Luiza = new Pessoa();
            $Luiza->Falar();
        ~~~

<div id = "modificadoresdeacesso"></div>

### **Níveis de visibilidade / Modificadores de acesso:**
Os níveis de visibilidade, tem a função de gerir se é possível ou não acessar determinados métodos ou atributos dependendo da circunstância:

* `public` é possível acessar os atributos ou métodos que tem esse nível em qualquer circunstância;

*  `private` somente a classe criadora que pode acessar o método ou atributo que tenha esse nível;

* `protected` somente a classe criadora e as que herdam dela podem acessar o método ou atributo que tenha esse nível;

*OBS: Caso tenha duvida sobre herança, mais a frente será explicado como funciona essa mecânica da linguagem.*

<div id = "return"></div>

### **Retornando algo nos métodos:**

Assim como as funções os métodos podem retornar valores, porém para isso é necessário a clausula `return`:

- Sintaxe:
    ~~~php
        class <nomeDaClasse>
        {
            <nivelDeVisibilidade> <nomeDoMétodo>($<parametros>)
            {
                return <oQueDesejaRetornar>;
            }
        }
    ~~~

- Exemplo:
    ~~~php
        class Calculadora
        {
            public Somar($numero1, $numero2)
            {
                return $numero1 + $numero2;
            }
        }
    ~~~

<div id = "getset"></div>

### **Métodos GETTERS e SETTERS:**

Esses métodos são criados para acessar ou atribuir valores a atributos que são privados:

<div id = "get"></div>

- **GETTERS:**
    
    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                private $<atributo>;

                public function getAtributo()
                {
                    return $this->atributo;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Cadastro
            {
                private login;
                private senha;

                public function getSenha()
                {
                    return $this->senha;
                }
            }
        ~~~

<div id = "set"></div>

- **SETTERS:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                private $<atributo>;

                public function setAtributo($<variavelParaReceberOValor>)
                {
                    $this->atributo = $<variavelParaReceberOValor>;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Cadastro
            {
                private login;
                private senha;

                public function setSenha($senha)
                {
                    $this->senha = $senha;
                }
            }
        ~~~

<div id = "construtor"></div>

### **Construtor ( __construct ):**

É um método que é inicializado quando instanciamos sua classe.

- Sintaxe:
    ~~~php
        class <nomeDaClasse>
        {
            public function __construct($<parametros>)
            {
                ...
            }
        }
        
    ~~~

- Exemplo:
    ~~~php
        class Pessoa
        {
            private $nome;
            private $idade;
            private $RG;

            public function __construct($nome, $idade, $RG)
            {
                $this->nome = $nome;
                $this->idade = $idade;
                $this->RG = $RG;
            }
        }

        $p = new Pessoa("Paulo", 29, "11111111111");
    ~~~

<div id = "heranca"></div>

### **Herança (extends):**

Herança em POO tem a função de "puxar" os atributos e métodos da classe herdada para a classe herdeira:

- Sintaxe:
    
    ~~~php
        class <nomeDaClasseQueSeraHerdada>
        {
            <nivelDeVisibilidade> $<atributos>;
            ...
            <nivelDeVisibilidade> function <nomeDoMétodo>()
            {
                ...
            }
        }

        class <nomeDaClasseHerdeira> extends <nomeDaClasseQueSeraHerdada>
        {
            ...
        }
    ~~~

- Exemplo:
    
    ~~~php
        class Veiculo
        {
            public $cor;
            public $ano;
            public $nome;
            public function rodar()
            {
                echo "rodou";
            }
        }

        class Carro extends Veiculo //Nesse caso a classe Carro herda todos os atributos e métodos da classe veiculo.
        {
            public function ligarLimpador()
            {
                echo "Limpando o parabrisa";
            }
        }
    ~~~

<div id = "abstract"></div>

### **Abstração(abstract):**

Em POO algo abstrato é como se fosse um modelo a ser seguido:

<div id = "abstractclass"></div>

- **Classe abastrata:**

    - Uma classe abstrata só pode ser herdada, porém nunca instanciada:

        - Sintaxe:

            ~~~php
                abstract class <nomeDaClasse>
                {
                    ...
                }

                class <nomeDaClasse2> extends <nomeDaClasse>
                {
                    ...
                }
            ~~~

        * Exemplo:
            ~~~php
                abstract class Banco // Nesse caso o banco não pode ser instanciado, é apenas uma classe "molde"
                {
                    public function Depositar()
                    {
                        echo "depositou";
                    }
                }

                class BancoA extends Banco
                {
                }
            ~~~

<div id = "abstractmethod"></div>

- **Métodos Abstratos:**

    - Métodos abstratas seguem o mesmo conceito de "molde", porém eles obrigam que as classes que herdarem criem os métodos abstratos invocados:

        - Sintaxe:

            ~~~php
                abstract class <nomeDaClasse>
                {
                    abstract <nivelDeVisibilidade> function <nomeDoMétodo>($<parametros>);
                }

                class <nomeDaClasse2> extends <nomeDaClasse>
                {
                    <nivelDeVisibilidade> function <nomeDoMétodoAbstrato> ($<parametrosDoMétodoAbstrato>)
                    {
                        ...
                    }
                }
            ~~~

        * Exemplo:

            ~~~php
                abstract class Banco
                {
                    abstract public function Depositar($dinheiro);
                }

                class BancoA extends Banco
                {
                    public function Depositar($dinheiro) //Nesse caso o método depositar tem que ser implementado, visto que ele é abstrato
                    {
                        echo "Você depositou: ".$dinheiro;
                    }
                }
            ~~~

        *OBS: A classe que contem o método abstrato também tem que ser abstrata.*

<div id = "const"></div>

### **Como trabalhar com constantes em classes:**

<div id = "defineconst"></div>

- **Como definir um atributo constante:**
    
    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                const <nomeDoAtributo> = <valor>;
            }
        ~~~

    - Exemplo:
        ~~~php
            class Cachorro
            {
                const RACA = "Chow chow";
            }
        ~~~

<div id = "getconst"></div>

- **Como pegar um atributo constante (self::):**

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                const <nomeDoAtributo> = <valor>;

                <nivelDeVisibilidade> function <nomeDoMétodo>($<parametros>)
                {
                    return self::<nomeDoAtributo>;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Animal
            {
                const nome = "Lobo";

                public function pegarConst()
                {
                    return self::nome;
                }
            }
        ~~~

<div id = "parents"></div>

- **Como pegar um atributo constante da classe "Pai" da herança:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasseFilho>  extends <nomeDaClassePai>
            {
                const <nomeDoAtributo> = <valor>;

                <nivelDeVisibilidade> function <nomeDoMétodo>($<parametros>)
                {
                    return parent::<nomeDoAtributoDaClassePai>;
                }
            }
        ~~~

    - Exemplo:
        ~~~php
            class Animais
            {
                const nome = "Porco";
            }
            class Feras extends Animais
            {
                const nome = "Leao";

                public function pegarConst()
                {
                    return parent::nome; // irá retornar Porco.
                }
            }
        ~~~

<div id = "static"></div>

### **Estático (static):**

Atributos e métodos estáticos tem a caracteristica de não necessitarem ter sua classe instanciada para funcionar:

<div id = "atributesstatic"></div>

- **Atributos estáticos:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                <nivelDeVisibilidade> static        $<nomeDOatributo>;
            }

            <nomeDaClasse>::$<nomeDoAtributo>;
        ~~~

    - Exemplo:
        ~~~php
            class Login
            {
                public static $user;
            }

            Login::$user = "admin";
        ~~~
    
    *OBS: Caso você deseje utilizar um atributo static em um método é necessário utilizar o `self::$<nomeDoAtributo>`*

<div id = "methodstatic"></div>

- **Métodos estáticos:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                <nivelDeVisibilidade> static function <nomeDoMétodo>($<parametros>)
                {
                    ...
                }
            }

            <nomeDaClasse>::<nomeDoMétodo>();
        ~~~

    - Exemplo:
        ~~~php
            class Ferramentas
            {
                public static function Printar($mensagem)
                {
                    echo $mensagem;
                }
            }

            Ferramentas::Printar("Ola mundo");
        ~~~
    
<div id = "polimorfismo"></div>

### **Polimorfismo:**

O polimorfismo é quando sobrescrevemos um método que já havia sido herdado:

- Sintaxe:
    ~~~php
        class <nomeDaClasse1>
        {
            <nivelDeVisibilidade> function <nomeDoMétodo>()
            {
                ...
            }
        }

        class <nomeDaClasse2> extends <nomeDaClasse1>
        {
            <nivelDeVisibilidade> function <nomeDoMétodoQueSeráSobrescrito>()
            {
                ...
            }
        }
    ~~~

- Exemplo:
    ~~~php
        class Animal
        {
            public function Andar()
            {
                echo "andou";
            }
        }

        class Vaca extends Animal
        {
            public function Andar()
            {
                echo "a vaca andou";
            }
        }
    ~~~

<div id = "interface"></div>

### **Interfaces:**
Interface tem a função de criar um molde de métodos, assim quer "herdar"/implementar ela, será obrigado a cria-los.

- Sintaxe:
    ~~~php
        interface <nomeDaInterface>
        {
            <nivelDeVisibilidade> function <nomeDoMétodo>($<parametros>);
            ...
        }

        class <nomeDaClasse> implements <nomeDaInterface>
        {
            <nivelDeVisibilidade> function <nomeDoMétodo>($<parametros>);
            ...
        }
    ~~~

- Exemplo:
    ~~~php
        interface Crud
        {
            public function create();
        }

        class Noticias implements Crud
        {
            public function create()
            {
                echo "criado";
            }
            public function teste()
            {
                echo "teste";
            }
        }
    ~~~

<div id = "namespace"></div>

### **Namespace:**

- O nasmespace tem a função de diferenciar classes que possuam o mesmo nome, sendo assim é como se fosse um indentificador:

<div id = "createnamespace"></div>

- **Como criar um namespace:**

    - Sintaxe:
        ~~~php
            namespace <nomeDoNamespace>;
            class <nomeDaClasse>
            {
                ...
            }
        ~~~

    - Exemplo:
        ~~~php
            namespace classe1;
            class Produto
            {
                public function teste()
                {
                    echo "Classe1";
                }
            }

            namespace classe2;
            class Produto
            {
                public function teste()
                {
                    echo "Classe2";
                }
            }
        ~~~

    *OBS: Todo o conteudo que ficar abaixo do namespace será considerado parte dele, até que encontre outro namespace, ou o código termine.*

<div id = "usetwopoints"></div>

- **Como utilizar o namespace com as " \ ":**

    - Sintaxe:
        ~~~php
            $<nomeDaVariavel> = new \<nomeDoNamespace>\<NomeDaClasse>();
        ~~~

    - Exemplo:
        ~~~php
            $p = new \classe1\Produto();
        ~~~

<div id = "use"></div>

- **Como utilizar o namespace com o "use":**

    - Sitaxe:
        ~~~php
            use <nomeDoNamespace>\<nomeDaClasse>;
        ~~~

    - Sintaxe:
        ~~~php
            use classe1\Produto();
        ~~~

<div id = "namenamespace"></div>

- **Como apelidar um namespace:**

    - Sintaxe:
        ~~~php
            use <nomeDoNamespace>\<nomeDaClasse> as <apelido>;
        ~~~

    - Exemplo:
        ~~~php
            use classe1\Produto as Produto1;
        ~~~

    *OBS: Nesse caso você terá que instanciar o apelido da classe*.

<div id = "referenceclone"></div>

### **Referencia e clonagem de objetos:**

Quando atribuimos uma instancia a uma nova variavel, será criado uma atribuição por referencia, fazendo assim que ambos compartilhem as mesmas caracteristicas (caso um seja alterado, o outro também será).

Seguindo essa lógica o php nos permite transformar essa atribuição por referencia, para uma atribuição por dado (clonar):

<div id = "clone"></div>

- **Clone:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                ...
            }

            $<variavelA> = new <nomeDaClasse>();
            $<variavelB> = clone $<variavelA>;
        ~~~

    - Exemplo:
        ~~~php
            class Pessoa
            {
                public $idade;
            }

            $a = new Pessoa();
            $b = clone $a;
            $b->idade = 20; //assim somente o b será alterado
        ~~~

<div id = "mclone"></div>

- **Método mágico clone( __clone ):**

    - Esse método será chamado sempre que exitir o clone antes da instancia da classe que o contem:

    - Sintaxe:
        ~~~php
            class <nomeDaClasse>
            {
                <nivelDeVisibilidade> function __clone()
                {
                    ...
                }
                ...
            }
        ~~~

    - Exemplo:
        ~~~php
            class Pessoa
            {
                public function __clone()
                {
                    echo "clonou";
                }
            }

            $a = new Pessoa();
            $b = clone $a;
        ~~~

<div id = "excecao"></div>

### **Tratamento de exceções:**

<div id = "exception"></div>

- **Como gerar uma exceção:**

    - Sintaxe:
        ~~~php
            throw new Exception("<AMensagemDaExceção>", <codigoQueDeseja>);
        ~~~

    - Exemplo:
        ~~~php
            throw new Exception("Erro de cadastro", 1);
        ~~~

<div id = "trycatch"></div>

- **Como tratar um exceção com try catch:**

    - Sintaxe:
        ~~~php
            try
            {
                <codigoQuePodeDarErro>
            }catch(Exception $<nomeDaVariavelQueVaiConterOsDadosDaExeção>){
                <codigoQueVaiRodarSeDerErro>
            }
        ~~~

    - Exemplo:
        ~~~php
            try
            {
                $v = 1;
                if ($v != 1)
                {
                    throw new Exception("Valor diferente de 1", 101);
                }
                else
                {
                    echo $v;
                }
            }catch(Exception $v2){
                echo $v2->getMessage();
            }
        ~~~

<div id = "metodosexception"></div>

- **Métodos do Exception:**

    - `getMessage()` ===> pega a mensagem da exeção;
    - `getCode()` ===> pega o codigo da exeção;
    - `getFile()` ===> pega o path do arquivo que está dando o erro;
    - `getLine()` ===> pega a linha que deu a exeção.

<div id = "relacoes"></div>

### **Relação entre os objetos:**

Uma relação entre objetos é uma forma de conseguirmos acessar dados de um objeto dentro de outro:

<div id = "associacao"></div>

- **Associação:**

    - Sintaxe:
        ~~~php
            class <nomeDaClasseA>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoDaClasseB>;
            }

            class <nomeDaClasseB>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoB>;
            }

            $<variavelA> = new <NomeDaClasseA>();
            $<variavelA>-><nomeDoAtributoDaClasseB>-><nomeDoAtributoB>;
        ~~~

    - Exemplo:
        ~~~php
            class ClasseA
            {
                public $associacaoB;
            }
            class ClasseB
            {
                public $B;
            }

            $a = new ClasseA();
            $b = new ClasseB();
            $b->B = "Ola mundo";
            $a->associacaoB = $b;
            echo $a->associacaoB->B;
        ~~~

<div id = "agregacao"></div>

- **Agregação:**

    - Ocorre quando uma classe precisa de outra para fazer alguma ação, sendo assim inializamos ela com um construtor:

    - Sintaxe:
        ~~~php
            class <nomeDaClasseA>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoAgregadoAClasseB>;

                function __construct(<tipoDaClasse B> $<parametro>)
                {
                    $this-><nomeDoAtributoAgregadoAClasseB> = $<parametro>;
                }
            }

            class <nomeDaClasseB>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoB>;
                function __construct($<parametro>)
                {
                    $this-><nomeDoAtributoB> = $<parametro>;
                }
            }
            $<variavelB> = new <nomeDaClasseB>($<parametos>)
            $<variavelA> = new <NomeDaClasseA>($<variavelB>);
        ~~~

    - Exemplo:
        ~~~php
            class Carrinho
            {
                public $produtos;

                function __construct(Produtos $p)
                {
                    $this->produtos[] = $p;
                }
            }

            class Produtos
            {
                public $nome;
                function __construct($nom)
                {
                    $this->nome = $nom;
                }
            }
            $toalha = new Produtos("toalha");
            $compras = new Carrinho($toalha);
        ~~~

<div id = "composicao"></div>

- **Composições:**

    - Na composição, uma classe cria a instancia de outra classe dentro de si, sendo assim, quando ela for destruida, a outra classe também será.

    - Sintaxe:
        ~~~php
            class <nomeDaClasseA>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoA>;
            }

            class <nomeDaClasseB>
            {
                <nivelDeVisibilidade> $<nomeDoAtributoBCompostoPorA>;
                function __construct($<parametro>)
                {
                    $this-><nomeDoAtributoBCompostoPorA> = $<parametro>;
                }
            }
            $<variavelB> = new <nomeDaClasseB>($<parametros>);
        ~~~

    - Exemplo:
        ~~~php
            class Pessoa
            {
                public $nome;
            }

            class Exibir
            {
                public $pessoa;
                function __construct($n)
                {
                    $this->pessoa = new Pessoa();
                    $this->pessoa->nome = $n;
                }
            }
            $e = new Exibir("Joao");
        ~~~

<div id = "metodosmagicos"></div>

### **Métodos mágicos:**

* *Os métodos mágicos devem ser utilizados dentro de uma classe*

* `__set($<atributoPrivado>, $<input>)` ===> funciona como uma maneira de criar sets, de maneira mais prática, uma vez que ao criar esse método o atributo poderá ser setado como se fosse publico.
* `__get($<atributoPrivado>)` ===> funciona como uma maneira de criar gets, de maneira mais prática, uma vez que ao criar esse método o atributo poderá ser pego como se fosse publico.
* `__tostring`===> esse método é chamado toda vez que o objeto for considerado como uma string;
* `__invoke`===> esse método é chamado toda vez que você tenta chamar um método como função.

* *OBS: Todos o métodos mágicos são criados como o __construct, sendo "public function <nomeDoMétodo>(){}"*

### **Namespace e Use:**

- **Como criar um namespace:**
    - Os namespaces é uma maneira de organizarmos nossas classes afim de não haver conflitos, caso existam duas classes com mesmo nome.

    - Sintaxe:
        ~~~php
            namespace <ElementoOrganizacional>\<NomeDoNamespace>;
        ~~~

    - Exemplo:
        ~~~php
            namespace MainPage\Login;
        ~~~

    OBS: Criando o namespace dessa maneira tudo que vier abaixo dele entra dentro do seu contexto.

    OBS2: O namespace funciona de maneira semelhante a pastas em SO.

- **Como utilizar o "use":**

    - O use tem a função de indicar qual é o namespace que você está utilizando, podendo apelida-los, caso queira.

    - Sintaxe:
        ~~~php
            use <nomeDoNamespace>\<ClasseDoNasmespaceQueDeseja>;
            ...
        ~~~

    - Exemplo:
        ~~~php
            use MainPage\Home;
        ~~~

    OBS: Diferente de outras linguagens o use não 'importa' o arquivo que contem a classe, sendo assim é necessário utilizar o require_once ou spl_autoload_register.

### **spl_autoload_register:**

- O spl_autoload_register tem a função de criar uma forma mais simples de fazer os require_once (além de fazer diversos require, ao mesmo tempo).

- Exemplo:
    ~~~php
            spl_autoload_register(function ($class) {
                require_once "$class.php";
            });
    ~~~

OBS: o sql_autoload_register() aceita uma function onde seu parametro será o nome das instancias;

<div id = "pootipado"></div>

## *Como utilizar o php POO tipado:*

<div id = "tipandopropriedades"></div>

### **Como tipar propriedades de classes:**

* Sintaxe:
    ~~~php
        class <nomeDaClasse>
        {
            <nivelDeVisibilidade> <tipoDaPropriedade> $<nomeDaPropriedade>;
        }
    ~~~

* Exemplo:
    ~~~php
        class Pessoa
        {
            public string $nome;
        }
    ~~~

<div id = "tipandometodoseretornos"></div>

### **Como tipar métodos e seus retornos:**

* Sintaxe:
    ~~~php
        class <nomeDaClasse>
        {
            <nivelDeVisibilidade> function <nomeDoMétodo>(<tipoDoArgumento> $<nomeDoArgumento>):<tipoDeRetorno>
            {
                ...
            }
        }
    ~~~

* Exemplo:
    ~~~php
        class Pessoa
        {
            public function Falar(string $frase):string
            {
                return "Frase: $frase";
            }
        }
    ~~~
