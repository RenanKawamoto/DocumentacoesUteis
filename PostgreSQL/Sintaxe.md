# **Sintaxe básica SQL:**

## **Sumário:**
1. [O que é SQL, DML e DQL](#significado)
2. [Operadores](#operadores)
    1. [Lógicos](#logicos)
    2. [Relacionais](#relacionais)
    3. [BETWEEN](#between)
    4. [LIKE](#like)
    5. [IN](#in)
3. [Funções de agregação](#agregacao)
4. [DML](#dml)
    1. [INSERT](#insert)
    2. [UPDATE](#update)
    3. [DELETE](#delete)
5. [DQL](#dql)
    1. [SELECT](#select)
    2. [WHERE](#where)
    3. [ORDER BY](#order)
    4. [GROUP BY](#group)
    5. [HAVING](#having)
    6. [DISTINCT](#distinct)
--------------------------------------

## **Nesse arquivo tentarei explicar de maneira breve a sintaxe utilizada no postgreSQL.**

--------------------------------------

<div id='significado'></div>

## *`-Significados:`*
* `SQL` = Linguagem de consulta estruturada;
* `DML` = Linguagem de manipulação de dados;
* `DQL` = Linguagem de consulta de dados;

<div id='operadores'></div>

## *`-Operadores:`*

<div id='logicos'></div>

### **Operadores lógicos:**
* `AND`: "e" lógico;
* `OR`: "ou" lógico;
* `NOT`: negação;

<div id='relacionais'></div>

### **Operadores relacionais:**
* `>`: maior que;
* `<`: menor que;
* `>=`: maior ou igual que;
* `<=`: menor ou igual que;
* `=`: igual à;
* `<>`: diferente de;

<div id='between'></div>

### **Operador BETWEEN:**
Especifica o intervalo entre um valor inicial e um final;

Exemplo:
~~~SQL
    idade BETWEEN 16 AND 21;
~~~
*Isso irá considerar os valores [16,17,18,19,20,21].*

OBS: Seria mesma coisa que fazer:
~~~SQL
    idade >= 16 AND idade <= 21;
~~~

<div id='like'></div>

### **Operador LIKE:**
Descreve se o campo é igual ao valor que vem em seguida do LIKE.

Exemplo:
~~~SQL
    nome LIKE 'Joao';
~~~
*O LIKE ve se o campo nome é igual a Joao.*

OBS: PODEMOS USAR O LIKE SEM TER QUE SABER CONTEUDO TOTAL, ATRAVÉS DO `%` (o % indica que aceita qualquer caracter ou caracteres).

Exemplo:
~~~SQL
    nome LIKE '%oao';
~~~
*Nesse caso o programa ira ver se campo termina com 'oao'.*

<div id='in'></div>

### **Operado IN:**
Descreve se o valor está dentro de uma lista.

Exemplo:
~~~SQL
    id IN (1,2,3,4,5,6)
~~~
*Nesse caso o IN tem a função de confirmar se o id é igual a algum elemento dessa lista de valores.*

<div id='agregacao'></div>

## *`-Funções de agregação:`*
* `AVG:` calcula a média;
* `COUNT:` retorna a quantidade de registros da seleção;
* `SUM:` retorna a soma de todos os valores de um determinado campo;
* `MAX:` retorna o maior valor de um determinado campo;
* `MIN:` retorna o menor valor de um determinado campo;

Como devem ser usados:
~~~SQL
    AVG(campoDaTabela)
~~~
OBS: Caso queira usar outra função agregada basta trocar seu nome e permanecer o ().

<div id='dml'></div>

<div id='insert'></div>

## *`-DML: Adicionr um registro na tabela (INSERT):`*

Sintaxe:
~~~SQL
    INSERT INTO <nomeDaTabela> <atributos(opcional)> VALUES <valores>;
~~~

Exemplo:

~~~SQL
    INSERT INTO clientes(nome, rg, cpf, dataDeNascimento) VALUES ('Pedro',00000000, 111111111, '01-01-1999');
~~~

<div id='update'></div>

### **DML: alterando dados de 1 registro da tabela (UPDATE):**

Sintaxe:
~~~SQL
    UPDATE <nomeDaTabela> SET <atributo = valor,> [CONDIÇÃO];
~~~

Exemplo:

~~~SQL
    UPDATE clientes SET nome='Joao', dataDeNascimento='02-02-1999' WHERE cpf=111111111;
~~~

<div id='delete'></div>

### **DML: apagando um registro da tabela (DELETE):**

Sintaxe:
~~~SQL
    DELETE FROM <nomeDaTabela> [CONDIÇÃO];
~~~

Exemplo:
~~~SQL
    DELETE FROM clientes WHERE cpf=111111111;
~~~

<div id='dql'></div>

<div id='select'></di>

## *`-DQL: Consultas (SELECT):`*

Sintaxe:
~~~SQL
    SELECT <campos> FROM <nomeDaTabela> [CONDIÇÃO];
~~~

Exemplo:
~~~SQL
    SELECT * FROM clientes;
~~~

Exemplo2:
~~~SQL
    SELECT nome, cpf FROM clientes;
~~~

<div id='where'></div>

### **DQL: cláusula WHERE:**
Essa cláusula adiciona condições da consulta.

Sintaxe:
~~~SQL
    SELECT <campos> FROM <nomeDaTabela> WHERE <condição>;
~~~

Exemplo:
~~~SQL
    SELECT * FROM clientes WHERE nome LIKE '%ao%';
~~~

<div id='order'></div>

### **DQL: cláusula ORDER BY:**
Essa cláusula é utilizada para ordenar os valores de acordo com determinado campo.

Sintaxe:
~~~SQL
    SELECT <campos> FROM <nomeDaTabela> ORDER BY <campos>;
~~~

Exemplo:
~~~SQL
    SELECT * FROM clientes ORDER BY nome;

    SELECT * FROM clientes ORDER BY idade DESC;

    SELECT * FROM clientes ORDER BY cadastro ASC, nome DESC;
~~~

OBS: como é possível observar no exemplo, a cláusula ORDER BY apresenta termos adicionais que causam impacto na ordenação, sendo eles ASC(mostra os valores de modo crecente) e DESC(mosta os valores de maneira decrecente).

<div id='group'></div>

### **DQL: cláusula GROUP BY:**
Essa cláusula tem a função de separar a consulta em grupos especificos.

Sintaxe:
~~~SQL
    SELECT <campos> FROM <nomeDaTabela> GROUP BY <campos>;
~~~

Exemplo:
~~~SQL
    SELECT dataDeNascimento, nome FROM clientes GROUP BY dataDeNascimento;
~~~

<div id='having'></div>

### **DQL: cláusula HAVING:**
Utilizada para expressar a condição que deve ser satisfazer o GROUP BY.

Sintaxe:
~~~SQL
    SELECT <campos> FROM <nomeDaTabela> GROUP BY <campos> HAVING <condição>;
~~~

Exemplo:
~~~SQL
    SELECT id_cliente, SUM(total) AS total_pedido
    FROM pedidos
    GROUP BY id_cliente
    HAVING COUNT(id) > 1;
~~~

<div id='distinct'></div>

### **DQL: cláusula DISTINCT:**
Seleciona dados sem repetição.

Sintaxe:
~~~SQL
    SELECT DISTINCT <campos> FROM <nomeDaTabela>;
~~~

Exemplo:
~~~SQL
    SELECT DISTINCT nome FROM clientes;
~~~

