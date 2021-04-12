# **Sintaxe básica SQL:**

## **Sumário:**
1. [Comandos do PSQL](#psql)
2. [O que é SQL, DML e DQL](#significado)
3. [Operadores](#operadores)
    1. [Lógicos](#logicos)
    2. [Relacionais](#relacionais)
    3. [BETWEEN](#between)
    4. [LIKE](#like)
    5. [IN](#in)
4. [Funções de agregação](#agregacao)
5. [DML Básico](#dml)
    1. [INSERT](#insert)
    2. [UPDATE](#update)
    3. [DELETE](#delete)
6. [DQL Básico](#dql)
    1. [SELECT](#select)
    2. [WHERE](#where)
    3. [ORDER BY](#order)
    4. [GROUP BY](#group)
    5. [HAVING](#having)
    6. [DISTINCT](#distinct)
7. [DQL com JOINS](#dqljoin)
    1. [INNER JOIN](#inner)
    2. [LEFT JOIN](#left)
    3. [RIGTH JOIN](#right)
8. [DQL com Subqueries](#sub)
9. [Criando foreign key](#fk)
10. [DML: TRANSACTIONS](#transactions)
--------------------------------------

## **Nesse arquivo tentarei explicar de maneira breve a sintaxe utilizada no postgreSQL.**

--------------------------------------

<div id='psql'></div>

## *`-Comandos uteis do psql:`*

Caso você não esteja utilizando o postegreSql com psql, pode pular essa sessão.

### **Logar em um banco de dados:**

Sintaxe:
~~~
    psql -U <nomeDoUsuario> -d <nomeDoBancoDeDados> -p <porta> -h <host>
~~~
*Nesse caso tudo que possui **-** antes de uma letra é uma **flag**(um argumento para o psql).*

OBS: Caso você não passe o -d ele irá entrar no data base padrão, o postgres / E caso você não passe o -U ele tentará entrar como o usuario da sua maquina.

Exemplo:
~~~
    psql -U postgres -d bancoDeTeste -p 5433 -h localhost
~~~

### **Dentro do psql, como ver os bancos:**

Sintaxe:
~~~
    \ls
~~~

### **Dentro do psql, como ver as tabelas:**

Sintaxe:
~~~
    \dt
~~~

### **Dentro do psql, como ver as tabelas, sequencias e views:**

Sintaxe:
~~~
    \d
~~~

### **Dentro do psql, como ver sair:**

Sintaxe:
~~~
    \q
~~~

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
    SELECT AVG(campoDaTabela) FROM tabela;
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
    INSERT INTO clientes(nome, rg, cpf, dataDeNascimento) VALUES ('Pedro','2222222', '111111111', '01-01-1999');
~~~

<div id='update'></div>

### **DML: alterando dados de 1 registro da tabela (UPDATE):**

Sintaxe:
~~~SQL
    UPDATE <nomeDaTabela> SET <atributo = valor,> [CONDIÇÃO];
~~~

Exemplo:

~~~SQL
    UPDATE clientes SET nome='Joao', dataDeNascimento='02-02-1999' WHERE cpf='111111111';
~~~

<div id='delete'></div>

### **DML: apagando um registro da tabela (DELETE):**

Sintaxe:
~~~SQL
    DELETE FROM <nomeDaTabela> [CONDIÇÃO];
~~~

Exemplo:
~~~SQL
    DELETE FROM clientes WHERE cpf='111111111';
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

<div id='dqljoin'></div>

## *`-DQL com JOINS:`*

Os joins tem a função de relacionar tabelas nas consultas.

OBS: As <tabelasPrincipais> são as que não apresentam o FK, já as <tabelasDependentes> são as que apresentam o FK.

#### **Tipos:**
* INNER JOIN;
* LEFT JOIN;
* RIGHT JOIN;

Convenções:
~~~SQL
    SELECT <atributos>
    FROM
        <tabelaPrincipal> AS p
    <JOIN> <tabelaDependente1> AS d1 ON <ligaçãoChavePkFk>
    <JOIN> <tabelaDependente2> AS d2 ON <ligaçãoChavePkFk>
~~~

<div id='inner'></div>

### **INNER JOIN:**
* É um join em que é obrigatorio ter relacionemento entre as tabelas.
* A ligação deve ser entre constraints().

Sintaxe:
~~~SQL
    SELECT 
        <atributosDaTabelaPrincipal>,
        <atributoDaTabelaDependente> AS <nomeQueDesejar>(opcional)
    FROM
        <tabelaDependente>
    INNER JOIN
        <tabelaPricipal> ON <condicional>;
~~~

Exemplo:
~~~SQL
    SELECT 
        roles.name As role
        users.id,
    FROM
        users
    INNER JOIN
        roles ON users.role_id = roles.id;
~~~

<div id='left'></div>

### **LEFT JOIN:**
* Pode ter ou não relacionamento entre as tabelas;
* Direita é a tabela principal, esquerda é a tabela dependente;
* Os dados da esquerda são apresentado, mesmo não tendo relação.

Sintaxe:
~~~SQL
    SELECT 
        <atributosDaTabelaPrincipal>,
        <atributoDaTabelaDependente> AS <nomeQueDesejar>(opcional)
    FROM
        <tabelaPricipal>
        
    LEFT JOIN
        <tabelaDependente> ON <condicional>;
~~~

Exemplo:
~~~SQL
    SELECT 
        adresses.city,
        people.name AS nome
    FROM
        adresses
    LEFT JOIN
        people ON people.adress_id = adresses.id;
~~~

<div id='right'></div>

### **RIGHT JOIN:**
* Pode ter ou não relacionamento entre as tabelas;
* Direita é a tabela principal, esquerda é a tabela dependente;
* Os dados da direita são apresentado, mesmo não tendo relação.

Sintaxe:
~~~SQL
    SELECT 
        <atributosDaTabelaPrincipal>,
        <atributoDaTabelaDependente> AS <nomeQueDesejar>(opcional)
    FROM
        <tabelaPricipal>
        
    LEFT JOIN
        <tabelaDependente> ON <condicional>;
~~~

Exemplo:
~~~SQL
    SELECT 
        adresses.city,
        people.name AS nome
    FROM
        adresses
    RIGHT JOIN
        people ON people.adress_id = adresses.id;
~~~

<div id='sub'></div>

## *`-Subqueries:`*
* USE COM MODERAÇÃO;
* Um SELECT que foi "embutido" em outro comando como SELECT, UPDATE, DELETE ou dentro de outra subquery.

Sintaxe:
~~~SQL
    SELECT 
        *
    FROM
        <tabela>
    WHERE
        <atributo> = (<OutoSelect>)
~~~

Exemplo:
~~~SQL
    SELECT 
        *
    FROM
        people
    WHERE 
        people.id = (SELECT SUM(id) from people where people.id between 11 and 12);
~~~

<div id='fk'></div>

## *`-Foreign key:`*
São chaves, vindas de outras tabelas, que fazem uma ligação simples.

Sintaxe:
~~~SQL
    CREATE TABLE <nomeDaTabela>
    (
        <nomeDasPropriedades> <tipoDasPropriedasdes>,
        <nomeDaForeignKey_id> <tipoDaForeignKey>

        FOREIGN KEY (<nomeDaForeignKey_id>) REFERENCES <nomeDaTabelaQueAForeignKeyVeio>(<colunaDaPrimaryKey>)
    );
~~~

Exemplo:
~~~SQL
    CREATE TABLE people
    (
        name VARCHAR(100),
        address_id int

        FOREIGN KEY (address_id) REFERENCES adresses (id)
    );
~~~

<div id='transactions'></div>

## *`-TRANSACTIONS:`*
As transações possuem a função de rodar um código SQL somente se ele de certo por completo, se der algum erro ele não irá rodar.

Sintaxe:
~~~SQL
    BEGIN;
    <alteracao>
    <condicional>;
    COMMIT;
~~~

Exemplo:
~~~SQL
    BEGIN;
    UPDATE people SET cpf = '11111'
    WHERE id = 23;
    COMMIT;
~~~

### **ROLL BACK:**
Caso você decida que não quer fazer as alterações é possível colocar a cláusula `ROLLBACK` em vez da COMMIT, assim cancelando todas as alterações.

Sintaxe:
~~~SQL
    BEGIN;
    <alteracao>
    <condicional>;
    ROLLBACK;
~~~

Exemplo:
~~~SQL
    BEGIN;
    UPDATE people SET cpf = '11111'
    WHERE id = 23;
    ROLLBACK;
~~~

### **SAVEPOINT:**
É possível transformar a transaction em algo mais fracionado através da clausula `SAVEPOINT`, que salva todas as alterações vindas antes dela em um espaço especifico, assim podemos retornar a ele ou commita-lo.

Sintaxe:
~~~SQL
    BEGIN;
    <alteracao>
    <condicional>;
    SAVEPOINT <nomeDoSave>
    ROLLBACK;
~~~

Exemplo:
~~~SQL
    BEGIN;
    UPDATE people SET cpf = '11111'
    WHERE id = 23;
    SAVEPOINT alteracaoPessoa
    ROLLBACK TO alteracaoPessoa;
~~~

## *`-WINDOW FUNCTIONS:`*

É uma maneira de fazer com que as funções agregadoras sejam expressadas por linha, não por conjunto total.

Sintaxe:
~~~SQL
    SELECT <atributo1>, <atributo2>, funçãoagregadora(<atributo3>) OVER(PARTITION BY <atributo>) FROM <tabela>;
~~~

Exemplo:
~~~SQL
    SELECT name, age, avg(age) OVER(PARTITION BY name) FROM people;
~~~

## *`-VIEW:`*

Views são selects que você pode montar, para falicitar futuras consultas.

Sintaxe:
~~~SQL
    CREATE VIEW <nomeDaView> AS
        <selectDesejado>;
    SELECT * FROM <nomeDaView>; --Caso queira utilizar a view;
~~~

Exemplo:
~~~SQL
    CREATE VIEW nomeOrdenado AS
        SELECT name 
        FROM pessoa 
        ORDER BY name;
    SELECT * FROM nomeOrdenado;
~~~





