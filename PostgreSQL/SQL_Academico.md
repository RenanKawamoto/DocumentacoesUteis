# **Documentação SQL:**

## **Sumário:**
1.
2.
3.
4.

## **- Cap.1: Estrutura Lexical (Lexical Structure):**

**A entrada SQL consiste em uma sequência de comandos. Um comando é composto por uma sequência de tokens, terminados por um ponto e vírgula ( “ ; ” )**.O final do fluxo de entrada também encerra um comando. Quais tokens são válidos depende da sintaxe do comando específico.

Um token pode ser uma palavra-chave, um identificador, um identificador entre aspas, um literal (ou constante) ou um símbolo de caractere especial. Os tokens são normalmente separados por um espaço em branco (espaço, tabulação, nova linha), mas não precisam ser se não houver ambiguidade (o que geralmente é o caso apenas se um caractere especial for adjacente a algum outro tipo de token).

Além disso, **comentários podem ocorrer na entrada SQL. Eles não são tokens, eles são efetivamente equivalentes a espaços em branco.**

**A sintaxe SQL não é muito consistente em relação a quais tokens identificam comandos e quais são operandos ou parâmetros.**

-------------------------------------------

### **`1.1: Identificadores e palavras-chave (Identifiers and Key Words):`**

* Tokens como *SELECT, DELETE, DROP, VALUES, etc* são as chamadas **palavras-chave(key words)**.

* Os Tokens que nós definimos de alguma maneira, como por exemplo: *nome_da_minha_table, nome_do_meu_atributo, nome_do_meu_banco_de_dados, etc* são os conhecidos **identificadores(identifiers).** 

*OBS:  Como os identificadores dão os nomes para as tabelas, colunas ou outros objetos de banco de dados, dependendo do comando em que são usados. Às vezes são simplesmente chamados de “ nomes ”.*

Palavras-chave e identificadores têm a mesma estrutura lexical, o que significa que não se pode saber se um token é um identificador ou uma palavra-chave sem conhecer o idioma.

### *`-Padrão para nomear um identificado:`*

**Identificadores SQL e palavras-chave devem começar com uma letra ( a - z) ou um sublinhado ( _ ). Os caracteres subsequentes em um identificador ou palavra-chave podem ser letras, sublinhados, dígitos ( 0 - 9)** ou cifrões ( $ ). Observe que **cifrões não são permitidos em identificadores de acordo com a letra do padrão SQL, portanto, seu uso pode tornar os aplicativos menos portáveis**. O padrão SQL não definirá uma palavra-chave que contenha dígitos ou comece ou termine com um sublinhado, portanto, os identificadores dessa forma estão protegidos contra possíveis conflitos com extensões futuras do padrão.

**Os identificadores tem um limite de tamanho de 63 bytes.**

**Palavras-chave e identificadores sem aspas não diferenciam maiúsculas de minúsculas.**

### *`-Convenção:`*

Uma convenção frequentemente usada é escrever **palavras-chave em maiúsculas e nomes em minúsculas**

### *`-Identificador delimitado/entre aspas:`*

Existe um segundo tipo de identificador: o **identificador delimitado ou identificador entre aspas**. Ele é formado por uma sequência arbitrária de caracteres entre aspas duplas ( " ). **Um identificador delimitado é sempre um identificador, nunca uma palavra-chave**. Portanto, "select" pode ser usado para se referir a uma coluna ou tabela chamada “select”.

Os identificadores entre aspas podem conter qualquer caractere, exceto o caractere com código zero.Isso permite construir nomes de tabelas ou colunas que de outra forma não seriam possíveis, como aqueles contendo espaços ou "e" comercial. A limitação de comprimento ainda se aplica.

Uma **variante dos identificadores entre aspas permite incluir caracteres Unicode**. Esta variante começa com **U&(U maiúsculo ou minúsculo seguido de "e" comercial)** imediatamente antes da aspa dupla de abertura, sem nenhum espaço entre eles, por exemplo U&"foo". (Observe que isso cria uma ambiguidade com o operador &. Portanto use espaços ao redor do operador para evitar esse problema.) Dentro das aspas, os caracteres Unicode podem ser especificados na forma de escape escrevendo uma barra invertida seguida pelo número do ponto de código hexadecimal de quatro dígitos ou alternativamente uma barra invertida seguida por um sinal de mais seguido por um número de ponto de código hexadecimal de seis dígitos.

Exemplo:
~~~SQL
    U&"d\0061t\+000061"
~~~
*Isso irá criar a palavra "data".*

Caso você deseje trocar a forma de escape (que no caso é a " \ "), basta utilizar o código UESCAPE.

Exemplo:
~~~SQL
    U&"d!0061t!+000061" UESCAPE '!'
~~~

OBS: O caractere de escape pode ser qualquer caractere único diferente de um dígito hexadecimal, o sinal de mais, uma aspa simples, aspas duplas ou um caractere de espaço em branco. Observe que o caractere de escape é escrito entre aspas simples, não aspas duplas.

Para incluir o caractere de escape no identificador literalmente, escreva-o duas vezes.

Exemplo:
~~~SQL
    U&"\\"
~~~
*Isso é igual a uma barra*

-------------------------------------------

### **`1.2: Constantes (CONSTANTS):`**

Existem três tipos de constantes digitadas implicitamente no PostgreSQL : strings, strings de bits e números. As constantes também podem ser especificadas com tipos explícitos, o que pode permitir uma representação mais precisa e um manuseio mais eficiente pelo sistema. 

### *`-Constantes de String (String Constants):`*

Uma **constante de string em SQL é uma sequência arbitrária de caracteres delimitada por aspas simples ( ' )**, por exemplo 'This is a string'. 

Para incluir um caractere de aspas simples em uma constante de string, escreva duas aspas simples adjacentes, por exemplo 'Dianne''s horse'.

Duas constantes de string que são separadas apenas por um espaço em branco com pelo menos uma nova linha são concatenadas e efetivamente tratadas como se a string tivesse sido escrita como uma constante.

Exemplo:
~~~SQL
    SELECT 'nome'
    'Da'
    'Pessoa'
~~~
*Isso seria igual a SELECT 'nomeDaPessoa'*

Porém caso não exista uma quebra de linha ocorrerá um erro:

Exemplo:
~~~SQL
    SELECT 'nome' 'Da' 'Pessoa'
~~~
*Irá gerar um erro*

### *`-Constantes de string com escapes de estilo C:`*

Uma constante de string de escape é especificada escrevendo a letra E(maiúscula ou minúscula) logo antes da aspa simples de abertura, por exemplo E'foo'.

Exemplo:
~~~SQL
    E'\nquebra\nde\nlinha'
~~~

### *`-Constantes de string citadas em cifrão:`*

o PostgreSQL fornece outra maneira, chamada de “ citação do dólar ” , para escrever constantes de string. A constante string de dólares consiste em um sinal de dólar ( $ ), um opcional “ tag ” de zero ou mais caracteres, outro sinal de dólar, uma seqüência arbitrária de caracteres que compõe o conteúdo corda, um sinal de dólar, a mesma marca que começou esta citação de dólar e um cifrão.

Exemplo:

~~~SQL
    $$Frase qualquer ' \$$
~~~

OBS: nenhum caractere dentro de uma string entre aspas com um dólar é escapado: o conteúdo da string é sempre escrito literalmente. 
Exemplo.2:
~~~SQL
    $ function $
BEGIN
    RETURN ($ 1 ~ $ q $ [\ t \ r \ n \ v \\] $ q $);
END;
$ function $
~~~

### *`-Constantes de sequência de bits:`*
Constantes de cadeia de bits parecem constantes regulares iniciadas com um B(maiúscula ou minúscula) imediatamente antes da (espaços em branco não intervir) citação de abertura, por exemplo, B'1001'. Os únicos caracteres permitidos nas constantes de string de bits são 0e 1.

Como alternativa, as constantes de string de bits podem ser especificadas em notação hexadecimal, usando uma inicial X(maiúscula ou minúscula), por exemplo X'1FF',. Essa notação é equivalente a uma constante de string de bits com quatro dígitos binários para cada dígito hexadecimal.

### *`-Constantes Numéricas:`*

As constantes numéricas são aceitas nestas formas gerais:

~~~SQL
    digits
    digits. [ digits] [ e [ + - ]digits ]
    [ digits] digits[ e [ + - ]digits ]
    digitse [ + - ]digits
~~~

Onde **digits** é um ou mais dígitos decimais (0 a 9). Algumas regras, que podem te ajudar:

1° - Pelo menos um dígito deve estar antes ou depois do ponto decimal, se for usado.

2° - Pelo menos um dígito deve seguir o marcador de expoente ( e ), se houver.

3° - Não pode haver espaços ou outros caracteres incorporados na constante.

OBS: Qualquer sinal de mais ou menos à esquerda não é realmente considerado parte da constante; é um operador aplicado à constante.

Exemplo:
~~~SQL
    42
    3.5
    4.
    0.001
    5e2
    1.925e-3
~~~

OBS: Uma constante numérica que não contém um ponto decimal nem um expoente é inicialmente presumida como tipo integerse(32 bits); caso contrário, presume-se que seja do tipo bigintse(64 bits); caso contrário, é considerado tipo numeric. Constantes que contêm pontos decimais e / ou expoentes são sempre inicialmente presumidos como do tipo numeric.

OBS.2:Quando necessário, você pode **forçar um valor numérico a ser interpretado como um tipo de dados específico**:

Exemplo: 
~~~SQL
    REAL '1.23'  
    1.23::REAL   
~~~

### *`-Operadores não padrões:`*

Um nome de operador é uma sequência de até NAMEDATALEN-1:

~~~
+ - * / <> = ~! @ #% ^ & | `?
~~~

No entanto, existem algumas restrições aos nomes das operadoras:

**--** e **/\*** não podem aparecer em nenhum lugar no nome de um operador, pois serão interpretados como o início de um comentário.

Um nome de operador de vários caracteres não pode terminar em + ou -, a menos que o nome também contenha pelo menos um destes caracteres:
~~~
    ~! @ #% ^ & | `?
~~~
*Por exemplo, **@-** é um nome de operador permitido, mas **\*-** não é. Essa restrição permite que o PostgreSQL analise consultas compatíveis com SQL sem exigir espaços entre os tokens.*

Ao trabalhar com nomes de operador não padrão SQL, geralmente você precisará separar os operadores adjacentes com espaços para evitar ambiguidade. Por exemplo, se você definiu um operador unário esquerdo denominado @, você não pode escrever X*@Y; você deve escrever X* @Y para garantir que o PostgreSQL o leia como dois nomes de operador, não um.

### *`-Caracteres especiais:`*

Alguns caracteres que não são alfanuméricos têm um significado especial diferente de ser um operador.

* Um cifrão ( $ ) seguido por dígitos é usado para representar um parâmetro posicional no corpo de uma definição de função ou uma instrução preparada. Em outros contextos, o cifrão pode ser parte de um identificador ou de uma constante de string entre aspas.

* Os parênteses ( () ) têm seu significado usual para agrupar expressões e impor precedência. Em alguns casos, os parênteses são necessários como parte da sintaxe fixa de um determinado comando SQL.

* Os colchetes ( [] ) são usados ​​para selecionar os elementos de uma matriz. 

* As vírgulas ( , ) são usadas em algumas construções sintáticas para separar os elementos de uma lista.

* O ponto-e-vírgula ( ; ) termina um comando SQL. Ele não pode aparecer em qualquer lugar dentro de um comando, exceto em uma constante de string ou identificador entre aspas.

* Os dois pontos ( : ) são usados ​​para selecionar “ fatias ” de matrizes. Em certos dialetos SQL (como Embedded SQL), os dois pontos são usados ​​para prefixar nomes de variáveis.

* O asterisco ( * ) é usado em alguns contextos para denotar todos os campos de uma linha da tabela ou valor composto. Ele também tem um significado especial quando usado como o argumento de uma função de agregação, ou seja, que o agregado não requer nenhum parâmetro explícito.

* O período ( . ) é usado em constantes numéricas e para separar nomes de esquemas, tabelas e colunas.

### *`-Comentários:`*

Um comentário é uma sequência de caracteres que começa com travessões duplos e se estende até o final da linha, por exemplo:

~~~SQL
    -- Isso é um comentario.
~~~

Alternativamente, comentários de bloco de estilo C podem ser usados:

~~~SQL
    /* Outro tipo de comentário */
~~~





