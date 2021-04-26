# **Documentação básica de php**

## *Sumário:*
1.
2.
3.

## *Sintaxe básica:*

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

### **Mostrando elementos html com o php:**

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

### **Tipos de dados escalares:**

Os tipos escalares são:
* String (que se trata de um conjunto de caracteres, que são definidos dentro das aspas duplas);
* int (trata-se de um numero inteiro);
* float (trata-se de um numero que tenha um ponto flutuante);
* boolean (trata-se de um valor true ou false);

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

### **Tipos de dados especiais:**

Os tipos de dados especiais são:
* NULL (um valor vazio);
* Resource (um tipo de dado que é utilizado para fazer referencia a recursos externos [Exemplo: Manipulação com o banco de dados, manipulação de arquivos e etc])

### **Diferença entre as aspas:**

* `'` são literais, sendo assim todo conteudo dentro dela será interpretado como uma string.

* `"` são interpretativas, sendo assim caso haja uma variavel ela irá entender e colocar seu conteudo.

### **Concatenação:**

No php utilizamos o `.` para concatenar as informações.

Exemplo:
~~~
    $nome = "Maria";
    echo 'Ola meu nome é '.$nome.' e tenho 23 anos';
~~~

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

### **Array (numerico):**

- **Como definir um array sem elementos iniciais:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray> = array();
        ~~~

    - Exemplo:
        ~~~php
            $carros = array();
        ~~~

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

- **Como printar um array:**

    - Sintaxe:
        ~~~php
            print_r($<nomeDaVariavelDoArray>)
        ~~~

    - Exemplo:
        ~~~php
            print_r($carros);
        ~~~

- **Como alterar os indices de um array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray> = array(<n°DoIndice> => <valor1>, <valor2>, ...);
        ~~~

    - Exemplo:
        ~~~php
            $carros = array(1 => "gol", "fusca", "bmw");
        ~~~

- **Como adicionar elementos no final do array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray>[] = <valorASerAdicionado>
        ~~~
    
    - Exemplo:
        ~~~php
            $nomes[] = "Luiz";
        ~~~

- **Como adicionar elementos com um indice no final do array:**

    - Sintaxe:
        ~~~php
            $<nomeDoArray>[<indice>] = <valorASerAdicionado>
        ~~~
    
    - Exemplo:
        ~~~php
            $nomes[5] = "Luiz";
        ~~~

- **Como printar um unico elemento do array:**

    - Sintaxe:
        ~~~php
            echo $<nomeDoArray>[<indice>];
        ~~~

    - Exemplo:
        ~~~php
            echo $nomes[0];
        ~~~

- **Como ver quantos elementos existem dentro do array:**

    - Sintaxe:
        ~~~php
            count($<nomeDoArray>);
        ~~~

    - Exemplo:
        ~~~php
            echo cout($nomes)
        ~~~

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
                echo pessoa;
            }
        ~~~

### **Array associativo:**

São iguais aos numericos, porém com a diferença que definimos os indices para serem strings.

   - Sintaxe:
        ~~~php
            $<nomeDoArray> = array(<chaveIndice> => <valor1>, <chaveIndice2> => <valor2>, ...);
        ~~~

    - Exemplo:
        ~~~php
            $pessoa = array("nome" => "Luiza", "idade" => 35);
        ~~~

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

### **Funções de arrays:**

* `is_array($array)` ==> verifica se o elemento entre os () é um array, retornando um boolean;

* `in_array($valor, $array)` ==> verifica se o valor passado está dentro do array, retornando um boolean.

* `array_keys($array)` ==> retorna um novo array com as chaves do array passado.

