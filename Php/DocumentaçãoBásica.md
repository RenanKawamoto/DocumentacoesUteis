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

- São iguais aos numericos, porém com a diferença que definimos os indices para serem strings.

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

### **Condicionais:**

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
        ~~~

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

### **Operadores aritméticos:**

* `Adição( + )`
* `Subtração( - )`
* `Multiplicação ( * )`
* `Divisão ( / )`
* `Módulo ( % )`
* `Exponenciação ( ** )`

### **Operadores de Incremento e Decremento:**

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

### **Operadores de atribuição:**

* `=` ---> é um operador de atribuição simples, sendo assim ele irá transferir o valor da sua direta para a variavel na sua esquerda.
* `-=` ---> é um operador de atribuição que irá decrementar o valor que está na esquerda ao valor da direita.
* `+=` ---> é um operador de atribuição que irá incrementar o valor que está na esquerda ao valor da direita.
* `*=` ---> é um operador de atribuição que irá multiplicar o valor que está na esquerda ao valor da direita.
* `/=` ---> é um operador de atribuição que irá dividir o valor que está na esquerda ao valor da direita.
* `%=` ---> é um operador de atribuição que irá pegar o resto da divisão do valor que está na esquerda ao valor da direita.

### **Operadores de comparação:**
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

### **Operadores lógicos:**
* `&&` ou `and` ---> retorna true se os dois lados derem true;
* `||` ou `or` ---> retorna true se um dos lados ou os dois derem true;
* `xor` ---> retorna true quando apenas uma das condições for verdadeira;
* `!` ---> inverte o estado lógico da expressão.

### **Estruturas de repetição:**

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

### **Como criar funções:**

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

### **Validações(Validate Filters):**

- `isset($_POST['<indice>'])` ===> retorna um boolean se existir o indice;



