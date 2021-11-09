# **Documentação básica sobre JS(Java Script):**

## *Sumário:*
1.
2.
3.

## *Comandos simples de window:*

### **- window.alert():**

- A comando window.alert aceita uma string(conjunto de caracteres) como parametro e tem a função de gerar um alerta no navegar que interrompe os processos até ser confimado:

- Exemplo:
    ~~~js
        <script>
            window.alert("Hello world!");
        </script>
    ~~~

### **- window.confirm():**

- Esse comando tem a função de gerar um alert de confimação (tendo a opção de "Ok" e a opção de "cancelar"):

- Exemplo:
    ~~~js
        <script>
            window.confirm("You are ok?");
        </script>
    ~~~

### **- window.prompt():**

- Esse comando tem a função de gerar um alert com input:

- Exemplo:
    ~~~js
        <script>
            window.prompt("What is your name?");
        </script>
    ~~~

## *Comentarios:*

- Os comentarios em js podem ser feitos de duas formas:
    - Comentario por linha com "`//`";
    - Comentario em bloco com "`/* */`";


## *Variaveis e tipos:*

- Em js uma variavel segue a mesma estrutura de sintatica para ser definida como em C, porém com dois tipos principais "`var`"(define uma variavel em escopo global ou funcional) e "`let`"(define uma variavel em um escopo de bloco).

- Já em relação aos tipos primitivos, os principais seriam:
    - number (todos os numeros, tanto int quanto float);
    - boolean (true ou false);
    - string (que podem ser com " ", ' ', ou \` \`);

- Porém os tipos em js podem ser também:
    - Object;
    - Function;
    - Undefined;

**OBS:** Para ajudar nessa questão o js tem uma função "`typeof()`", que nos indica o tipo de uma variavel.

## *Conversão de tipos:*

### **- Number.parse...():**

- Number.parseInt(variavel);
- Number.parseFloat(variavel); 
- Number(variavel);

### **- String:**

- String(variavel);
- variavel.toString();

## *Formatação e funções uteis de strings:*

### **- Template string:**

- O template string é uma maneira mais organizada de formatar suas strings, sem utilizar a concatenação com o "+":

- Exemplo:
    ~~~js
        <script>
            var wordString = "string";
            var words = `This string use template ${wordString}`;
        </script>
    ~~~

**`OBS:`** Para utilizar o template string é necessário utilizar a crase(" **\`** ")

### **- length:**

- O length é uma maneira de saber qual o tamanho de uma string:

- Exemplo:
    ~~~js
        <script>
            var word = "potato";
            var nWord = word.length;
        </script>
    ~~~

### **- toUpperCase():**

- O toUpperCase tem a função de transformar uma string normal e uma string maiuscula:

- Exemplo:
    ~~~js
        <script>
            var word = "potato";
            var wordUpper = word.toUpperCase();
        </script>
    ~~~

### **- toLowerCase():**

- O toLowerCase tem a função de transformar uma string normal e uma string minuscula:

- Exemplo:
    ~~~js
        <script>
            var word = "potato";
            var wordLower = word.toLowerCase();
        </script>
    ~~~

## *Formatação de number:*

### **- toFixed():**

- toFixed tem a função de restringir a quantidade de casas decimais de um numero float:

- Exemplo:
    ~~~js
        <script>
            var n = 10.52;
            var n2 = n.toFixed(1);
            window.alert(n2);
        </script>
    ~~~

## *Escrevendo informações no HTML:*

### **- document.write():**

- Esse método funciona semelhante ao "echo" do php, onde ele irá escrever dentro do html:

- Exemplo:
    ~~~js
        document.write("<h1>Hello World!!</h1>");
    ~~~

## *Operadores:*

### **- Aritméticos:**

- `+` = soma
- `-` = subtração
- `/` = divisão
- `*` = multiplicação
- `%` = divisão que pega o resto
- `**` = potenciação

### **- Auto atribuição:**

- `+=`
- `-=`
- `/=`
- `*=`
- `%=` 
- `**=`

### **- Relacionais:**

- `>` = maior
- `<` = menor
- `>=` = maior ou igual
- `<=` = menor ou igual
- `==` = igualdade de conteudo
- `!=` = diferença
- `===` = igualdade total

### **- Operadores lógicos:**

- `!` = negação
- `&&` = and
- `||` = or

### **- Operador ternario:**

- < testeLógico > ? < respostaSeForTrue > : < respostaSeForFalse >;

## *Introdução ao DOM:*

### **- O que é DOM:**

- DOM é um acronimo para Document Object Model, que se trata de um modelo de objetos para documentos, basicamento é um conjunto de objetos que te da acesso aos componentes internos do seu site.

### **- Qual é a estrura do DOM:**

- O DOM começa com sua raiz sendo o objeto `window`, que o pai de todos, assim o window tem como filhos location, document, history entre outro(cada um com uma respectiva função).

- Dentro do document existe html que se trata de toda a estrutura HTML presente no seu site, assim você pode acessa-los e altera-los.

### **- Como pegar elementos HTML pela tag com o DOM:**

- Para fazer esse processo basta percorrer a arvore DOM até onde desejar e utilizar o método getElementsByTagName():

- Exemplo:
    ~~~js
        <html>
            <head>
            ...
            <head>
            <body>
                <p>Hello world!</p>
                <p>Hello world2!</p>

                <script>
                    var p = window.document.body.getElementsByTagName('p')[0];
                </script>
            </body>
        </html>
    ~~~

### **- Como pegar um elemento HTML pelo id, class, name e seletores:**

- Igualmente ao mostrado com o comando getElementsByTagName(), é possível fazer utilizando  os outros:
    - `getElementsByName()`;
    - `getElementsById()`;
    - `getElementsByClass()`;
    - `querySelector(<elementoHTML># ou . <nomeDaClasse ou idDaClasse>)`;

### **- Como pegar o conteudo do HTML pelo DOM:**

- Caso você deseje pegar o conteudo de um elemento HTML com o DOM é muito simples, basta utilizar o comando `innerText`:

- Exemplo:
    ~~~js
        <html>
            <head>
            ...
            <head>
            <body>
                <p>Hello world!</p>
                <p>Hello world2!</p>

                <script>
                    var p = window.document.body.getElementsByTagName('p')[0].innerText;
                </script>
            </body>
        </html>
    ~~~

### **- Como alterar o CSS de um elemento HTML pelo DOM:**

- Caso você deseje alterar o CSS de um elemento HTML com o DOM é muito simples, basta utilizar o comando `style.`:

- Exemplo:
    ~~~js
        <html>
            <head>
            ...
            <head>
            <body>
                <p>Hello world!</p>
                <p>Hello world2!</p>

                <script>
                    var p = window.document.body.getElementsByTagName('p')[0];
                    p.style.color = 'blue';
                </script>
            </body>
        </html>
    ~~~

### **- Como pegar o HTML pelo DOM:**

- Caso você deseje pegar HTML de um elemento com o DOM é muito simples, basta utilizar o comando `innerHTML`:

- Exemplo:
    ~~~js
        <html>
            <head>
            ...
            <head>
            <body>
                <p><strong>Hello world!</strong></p>
                <p>Hello world2!</p>

                <script>
                    var p = window.document.body.getElementsByTagName('p')[0].innerHTML;
                </script>
            </body>
        </html>
    ~~~

### **- Eventos com o DOM:**

- No DOM é possível tratar eventos de duas maneiras: com a ajuda do HTML ou apenas com o JS, veremos a seguir os dois modos:
    - **Com a ajuda do HTML:**
        - Ao criar um elemento HTML é possível passar parametros, e alguns deles tem integração com o JS, usarei como exemplo o `onclick`:

        - Exemplo:
            ~~~js
                <html>
                    <head>
                    <head>
                    <body>
                        <button id='b' onclick = 'clicar()'>Hello</button>

                        <script>
                            function clicar()
                            {
                                window.document.body.querySelector('button#b').innerHTML = 'clicou';
                            }
                        </script>
                    </body>
                </html>
            ~~~

    - **Somente com o JS:**
        - Você pode criar alguns listeners para "escutar" os eventos de um determinado elemento utilizando o comando addEventListener('<nomeDoEvento>', <função>):

        - Exemplo:
            ~~~js
                <html>
                    <head>
                    <head>
                    <body>
                        <button id='b'>Hello</button>

                        <script>

                            var a = window.document.body.querySelector('button#b');

                            a.addEventListener('click', clicar)
                            function clicar()
                            {
                                window.document.body.querySelector('button#b').innerHTML = 'clicou';
                            }
                        </script>
                    </body>
                </html> 
            ~~~

## *Condicionais:*

### **- If / else if / else:**

- Em JS os condicionais funcionam da mesma maneira que nas outras linguagens baseadas em C:

- Exemplo:
    ~~~js
        var a = 12;
        if(a < 12)
        {
            window.console.log('menor que 12');
        }
        else if(a == 12)
        {
            window.console.log('igual a 12');
        }
        else
        {
            window.console.log('maior que 12');
        }
    ~~~

### **- Switch case:**

- Igualmente a estrutura do if, o switch funciona igual as linguagens baseada em C:

- Exemplo:
    ~~~js
        var b = 20;
        switch(b)
        {
            case 10:
                console.log('10');
                break;
            case 20:
                console.log('20');
                break;
            case 30:
                console.log('30');
                break;
            default:
                console.log('maior que 30');
                break;
        }
    ~~~

## *Repetições:*

### **- While:**

- Igual a em C:

- Exemplo:
    ~~~js
        var c = 6;
        while(c<10)
        {
            console.log(c);
            c++;
        }
    ~~~

### **- Do While:**

- Igual a em C:

- Exemplo:
    ~~~js
        var c = 10;
        do
        {
            console.log(c);
            c++;
        }while(c == 10)
    ~~~

### **- For:**

- Igual a em C:

- Exemplo:
    ~~~js
        for(var a=0; a < 10; a++)
        {
            console.log(a);
        }
    ~~~

## *Variaveis compostas:*

### **Array:**

- Em js para definir um array basta utilizarmos o "[]":

- Exemplo:
    ~~~js
        let num = [1,2,3,4,5];
        console.log(num);
    ~~~

- Para adicionar um elemento no final de um array basta utilizar o método `push()`:

- Exemplo:
    ~~~js
        let num = [1,2,3];
        num.push(4);
        console.log(num);
    ~~~

- Para pegar o tamanho de um array basta utilizar o atributo `length`:

- Exemplo:
    ~~~js
        let num = [1,2,3];
        console.log(num.length);
    ~~~

- Para colocar os elementos em ordem basta utilizar o método `sort()`:

- Para percorrer um vetor você pode usar o `for( in )`:

- Exemplo:
    ~~~js
        let num = [1,2,3];
        for(i in num)
        {
            console.log(num[i]);
        }
    ~~~

- Para saber se determinado valor está dentro do array basta utilizar o método `indeOf()`, que irá retornar o indice onde ele se encontra ou -1 caso não exista:

## *Funções:*

### **- Definindo uma função:**

- Para definir uma função em JS, é muito semelhante ao aprendido em php:

- Exemplo:
    ~~~js
        function nomeDaFuncao(var parametro)
        {
            console.log(parametro);
        }
    ~~~

## *Programação funcional com JS:*

### **- Funções de primeira classe:**

- Funções de primeira classe são como se fossem objetos, sendo assim podemos armazena-las em constantes/variaveis, passar funções como parâmetros em outras funções e até ter funções como retorno de outras funções:

- Exemplo:
    ~~~js
        const teste = function() { console.log("teste");};
    ~~~

### **- Funções callback:**

- Uma função callback é uma função passada a outra função como argumento, que é então invocado dentro da função externa para completar algum tipo de rotina ou ação.

- Exemplo:
    ~~~js
        function greeting(name) {
            alert('Hello ' + name);
        }

        function processUserInput(callback) {
            var name = prompt('Please enter your name.');
            callback(name);
        }

        processUserInput(greeting);
    ~~~

### **- Arrow Functions:**

- As Arrow functions é uma maneira de criar funções anonimas de maneira simplificada, sendo possível fazer de diversas maneiras, abordarei aqui algumas:

- Exemplo1:
    ~~~js
        // Caso você tenha uma função anonima com apenas uma linha de código
        var a = () => console.log("ola mundo");

        // Caso você tenha uma função anonima com mais de uma linha de código
        var b = () => {
            var z = "ola";
            console.log(z);
        }

        // Caso você tenha uma função com um unico parametro
        var c = nome => console.log(nome);

        // Caso você tenha uma função com varios parametros

        var d = (nome, idade) => console.log(nome + "\n" + "idade: " + idade);
    ~~~

**`OBS:`** Caso só tenha uma linha o código o programa entenderá que essa dever ser retornada.

### **- Imutabilidade:**

- A imutabilidade é um conceito que preza que os dados não podem ser alterados, sendo assim usamos eles como parametros para fazer uma alteração local, e não afetar outras partes do código.

### **- Rest operators:**

- É uma maneira simplificada de passar diversos argumentos para uma função, bastando utilizar os **`...`**:

- Exemplo:
    ~~~js
        function numbers(...nums)
        {
            for(i in nums)
            {
                console.log(nums[i]);
            }   
        }
    ~~~

**`OBS:`** o rest operator é entendido como sendo um array.

### **- Spread operator:**

- É uma forma de estender um array/objeto que é constante "herdando" seus valores:

- Exemplo:
    ~~~js
        const num = [1,2,3];
        const num2 = [...num, 4,5,6];

        const obj = {nome:"teste", idade:22};
        const obj2 = {...obj, sobrenome: "testado"}

        // também é possível alterar as propriedades com o spread
        const obj3 = {...obj, nome: "alterado"}
    ~~~

### **- Map:**

- Map é uma função recursiva que passa por cada um dos elementos presente em um array aplicando um função e retornando suas respostas:

- Exemplo:
    ~~~js
        const nums = [1,2,3];

        const dobleNums = nums.map((num) => num * 2);
        // [2,4,6], pois para cara elemento "num" do array será aplicado a regra presente na arrow function
    ~~~

### **- Filter:**

- Filter é uma função recursiva que só retorna os elementos do array que passarem por uma determinada regra definida:

- Exemplo:
    ~~~js
        const nums = [1,2,3];
        const oddNums = nums.filter(num => num % 2 != 0);
        // [1,3] pois esses são os numeros que seguem a regra defina pela arrow function
    ~~~

### **- Reduce:**

- Reduce é uma função recursiva que tem por objetivo retornar um unico elemento "reduzindo" o seu array:

- Exemplo:
    ~~~js
        const nums = [1,2,3,4,5];
        // Nesse caso o reduce ira pegar o valor anterior e somar com valor atual, e o segundo parametro passado é o valor anterior da posição 0
        const sumNums = nums.reduce((previous, current) => previous + current, 0);
    ~~~

### **- Promisse:**

- É um objeto usado para processamento assincrono. Uma Promisse representa um valor que pode estar disponivel agora, no futuro ou nunca.

- Toda promisse tem um "resolve" e um "reject", que podem ser acessadas pelos métodos then e catch respectivamente, vamos ver isso em um exemplo:

- Exemplo:
    ~~~js
        let prom = new Promise(
            (resolve, reject) => {
                let b = true;
                if(b)
                {
                    resolve('Success');
                }
                reject('Error');
            }
        );

        prom.then( (msg) => console.log(msg) ).catch( (error) => console.log(error) );
    ~~~

- Existem alguns métodos próprios das promises, um deles é o all(), que irá retornar uma função, apenas se todas as promises já tiverem rodado:

- Exemplo:
    ~~~js
        Promise.all(
            [
                promessa1,
                promessa2,
            ]
        ).then( () => console.log("Todas as promessas foram rodadas") ).catch( error => console.log(error));
    ~~~

- Outro método interessante das promises é o race(), que irá retornar o resultado presente na primeira promessa que for finalizada:

- Exemplo:
    ~~~js
        Promise.race([
            promessa1,
            promessa2,
        ]).then(message => console.log(message)).catch(error => console.log(error));
    ~~~

### **- Async:**

- É uma maneira simplificada de criar funções assincronas(que rodam em segundo plano), para isso basta utilizar o termo `async` antes da sua função:

- Exemplo:
    ~~~js
        async function teste()
        {
            var soma = 0;
            for (var i = 0; i < 100000000; i++)
            {
                soma += i;
            }
            console.log(soma+"\n-----------------");
        }
    ~~~

### **-  Async awaint:**

- O await é um termo que tem a função de "bloquear" a progressão do código enquanto a função dele não for finalizada:

- Exemplo:
    ~~~js
        var a = () => console.log("ola mundo");
        async function teste()
        {
            await a();
            console.log("Acabou o código");
        }
    ~~~

### **- High Order Functions:**

- As funções de alta ordem são funções que retornam outras funções, ou que são passadas por parametros:

- Exemplo:
    ~~~js
        function somar(x)
        {
            return (y) => x+y;
        }

        somar(5)(10); //15
    ~~~

