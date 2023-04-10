# **-Documentação básica de PostgreSQL**
--------------------------------------------------
## *-Sumário:*
1. [introducao](#introducao)
    1. [O que é PostgreSQL](#oqueeh)
    2. [Por que utilizar o PostgreSQL](#pqutilizar)
    3. [Convenções que serão utilizadas](#convencoes)
    4. [Fundamentos da arquitetura](#arquitetura)
2. [Configurando o postgreSQL no cmd](#configuracaocmd)
3. [Primeiro comando no cmd (psql)](#primeirocomandopsql)
4. [Como sair do psql](#sairpsql)
5. [Como criar um banco de dados](#criandodb)
6. [Como dropar um banco de dados](#dropdb)
7. [Linguagem SQL](#sql)
    1. [Criando e dropando tabelas](#cdtabelas)
    2. [Criando propriedade primary key](#primary)
    3. [Criando propriedade foreign key](#fk)
    4. [Inserindo dados em uma tabela (INSET VALUE)](#idadostabela)
    5. [Copiando tabela para um arquivo](#ctabelaparquivo)
    6. [Consultando uma tabela, de maneira simples (SELECT)](#stabela)
    7. [Consulta com join](#join)
    8. [Funções de agregacao](#agregacao)
    9. [UPDATE](#update)
    10. [DELETE](#delete)
8. [Conceitos um pouco mais avançados do SQL](#avancados)
    1. [VIEW](#view)
    2. [FOREIGN KEY](#ofk)
    3. [TRANSACTION](#transactions)
    4. [WINDOW FUNCTIONS](#window)
    5. [INHERITS](#heranca)
--------------------------------------------------
--------------------------------------------------

<div id = "introducao"></div>

<div id = "oqueeh"></id>

## *-O que é PostgreSQL:*
PostgreSQL é um sistema de gerenciamento de banco de dados relacional de objeto (ORDBMS [ É um sistema de gerenciamento de banco de dados (SGBD) semelhante a um banco de dados relacional, porém com um modelo de banco de dados orientado a objetos: objetos, classes e herança são suportados diretamente nos esquemas do banco de dados e na linguagem de consulta]) baseado no POSTGRES, Versão 4.2 , desenvolvido no Departamento de Ciência da Computação da Universidade da Califórnia em Berkeley.

--------------------------------------------------

<div id = "pqutilizar"></id>

## *-Por que utilizar o PostgreSQL:*
Pois é um sistema de gerenciamento de banco de dados relacional de objeto gratuito e que possui diversos recursos modernos, como por exemplo:
* Consultas complexas (Complex queries)
* Chave estrangeiras (Foreign keys)
* Gatilhos (Triggers)
* Integridade transacional (Transactional Integrity)
* Controle de simultaneidade multiversão (multiversion concunrrency control)

Aliado a isso, o PostgreSQL pode ser expandido por diversões caminhos como:
* tipos de dados (data types)
* funções (functions)
* operadores (operators)
* funções agregadas (aggregate functions)
* métodos de indice (index methods)
* linguagens procedurais (procedural languages)

--------------------------------------------------

<div id = "convencoes"></id>

## *-Convenções*
As seguintes convenções são usadas na sinopse de um comando: colchetes ( [and]) indicam partes opcionais. Chaves ( {and}) e linhas verticais ( | ) indicam que você deve escolher uma alternativa. Os pontos (...) significam que o elemento anterior pode ser repetido. Tudo que estiver entre ( ``variavel´´ ) é algo que você pode nomear da maneira que desejar.

Os comandos SQL são precedidos por "=>" e os comandos do shell são precedidos por "$".

Um administrador geralmente é uma pessoa encarregada de instalar e operar o servidor. Um usuário pode ser qualquer pessoa que está usando, ou deseja usar, qualquer parte do sistema PostgreSQL .

--------------------------------------------------

<div id = "arquitetura"></id>

## *-Fundamentos da arquitetura*
O PostgreSQL usa um modelo cliente / servidor (O modelo cliente-servidor (em inglês client/server model), em computação, é uma estrutura de aplicação distribuída que distribui as tarefas e cargas de trabalho entre os fornecedores de um recurso ou serviço, designados como servidores, e os requerentes dos serviços, designados como clientes.)

Sendo assim , uma sessão de PostgreSQL consiste nos seguintes processos de cooperação:
* Um processo de servidor, que gerencia os arquivos de banco de dados, aceitando conexões aplicativos clientes e executando ações no banco em nome dos clientes. (o servidor de banco de dados é chamado postgres).
* E um aplicativo cliente (frontend) que executar operações de banco de dados. Esses podem ser de natureza muito diversa: uma ferramenta orientada a texto, um aplicativo gráfico, um servidor da web que acessa o banco de dados para exibir páginas da web ou uma ferramenta especializada de manutenção de banco de dados. Alguns aplicativos cliente são fornecidos com a distribuição PostgreSQL (a maioria é desenvolvida por usuários.).

Como é típico dos aplicativos cliente / servidor, o cliente e o servidor podem estar em hosts diferentes. Nesse caso, eles se comunicam por meio de uma conexão de rede TCP/IP(é um conjunto de protocolos de comunicação entre computadores em rede). Você deve ter isso em mente, porque os arquivos que podem ser acessados ​​em uma máquina cliente podem não estar acessíveis (ou podem estar acessíveis apenas usando um nome de arquivo diferente) na máquina servidor de banco de dados.

O servidor PostgreSQL pode lidar com várias conexões simultâneas de clientes. Para isso, inicia ( “ bifurcações ” ) um novo processo para cada conexão. A partir desse ponto, o cliente e o novo processo do servidor se comunicam sem intervenção do processo original . Assim, o processo do servidor mestre está sempre em execução, aguardando as conexões do cliente, enquanto os processos do cliente e do servidor associado vêm e vão.

--------------------------------------------------

<div id = "configuracaocmd"></id>

## *-Configurando o ambiente para o desenvolvimento (Windows)*
Após a instalação do postgreSQL em sua maquina, é necessário configurar algumas variaveis de ambiente para funcionar os comando do banco de dados relacional de objetos no cmd do windows:

* Encontre a pasta ..\postegreSQ\versão\bin dentro de seus diretórios >> copie esse path (muitas vezes o diretorio está no seguinte caminho => C:\Program Files\PostgreSQL\13\bin):

![img0](https://uploaddeimagens.com.br/images/003/175/553/full/img0.png?1617140640)

* Vá na barra de pesquisa do windows >> Digite "variaveis de ambiente do sistema":

![img1](https://uploaddeimagens.com.br/images/003/175/253/full/img1.png?1617133038)

* Clique em "Variaveis de ambiente..." >> vá na parte de variaveis do sistema e selecione em "Path" >> clique em "Editar .." >> clique em "Novo" >> cole o path que você havia copiado >> de "Ok" em todas as telas até sair de todas.

![img2](https://uploaddeimagens.com.br/images/003/175/559/full/img2.png?1617140853)

--------------------------------------------------

<div id="primeirocomandopsql"></div>

## *-Como utilizar seu primeiro comando no prompt (psql):*
Para iniciarmos vamos utilizar um comando para abrir o psql no cmd:

~~~
    $ psql -h localhost -p 5433 -U postgres -d postgres
~~~
Explicando o comando:
* O comando psql, abrirá o postgreSQL como "cliente" em modo texto;
* "-h" se trata de uma flag, que indica qual o hostname que queremos conectar, nesse caso "localhost";
* "-p" é outra flag, que indica a porta que vamos utilizar, no caso "5433";
* "-U" mais uma flag, que indica o usuario que iremos usar, nesse caso "postgres" (caso você não coloque, ele ira utilizar o usuario do prompt que está aberto);
* "-d" uma flag opcional que indica qual banco iremos logar (OBS: caso não indique qual é, ele irá entrar no banco com o mesmo nome do usuario, no caso o postgres).

--------------------------------------------------

<div id="sairpsql"></div>

## *-Como sair do psql e voltar para o prompt:*

~~~
    \q
~~~

--------------------------------------------------

<div id="criandodb"></div>

## *-Como criar um banco de dados(data base):*

**Para criar apenas pelo cmd:**
~~~
    $createdb -h localhost -p 5433 -U postgres ``nomeDoBanco´´
~~~
*OBS: as flags possuem a mesma função do psql*

**Para criar dentro do psql:**
~~~
    => CREATE DATABASE ``nomeDoBanco´´;
~~~

OBS: O PostgreSQL permite que você crie qualquer número de bancos de dados em um determinado site. Os nomes dos bancos de dados devem ter um primeiro caractere alfabético e são limitados a 63 bytes de comprimento. Uma escolha conveniente é criar um banco de dados com o mesmo nome de seu nome de usuário atual. Muitas ferramentas assumem esse nome de banco de dados como o padrão, de modo que você pode economizar um pouco de digitação.

--------------------------------------------------

<div id="dropdb"></div>

## *-Como deletar/dropar um banco de dados(data base):*

**Para dropar apenas pelo cmd:**
~~~
    $dropdb -h localhost -p 5433 -U postgres nomeDoBanco
~~~

**Para dropar dentro do psql:**
~~~
    => DROP DATABASE ``nomeDoBanco´´;
~~~
--------------------------------------------------
--------------------------------------------------

<div id="sql"></div>

## **`-A linguagem SQL:`**
## *-Conceitos:*
PostgreSQL é um sistema de gerenciamento de banco de dados relacional ( RDBMS ). Isso significa que é um sistema de gerenciamento de dados armazenados nas relações. Relação é essencialmente um termo matemático para `tabelas (tables)`.

Cada tabela é uma coleção nomeada de `linhas`. Cada linha de uma determinada tabela possui o mesmo conjunto de `colunas` nomeadas e cada coluna possui um `tipo de dados específico`. Considerando que as colunas têm uma ordem fixa em cada linha, é importante lembrar que o SQL não garante a ordem das linhas na tabela de forma alguma (embora possam ser explicitamente classificadas para exibição).

As tabelas são agrupadas em `bancos de dados` e uma `coleção de bancos de dados gerenciados por uma única instância do servidor PostgreSQL` constitui um `cluster de banco de dados`.

OBS: todos os comandos passados aqui podem ser colocados em um arquivo .sql (CASO FAÇA ISSO, É NECESSÁRIO RODA-LOS COM O COMANDO:)
~~~SQL
    psql -U ``usuario´´ -d database -f ``nomeDoArquivo.sql´´
~~~

--------------------------------------------------

<div id="cdtabelas"></div>

## *-Criando e dropando tabelas:*
Para fazer esse procedimento é necessário que antes você esteja logado dentro de um banco de dados usando o psql.

### **-Criando uma tabela:**

Para criar uma tabela, devemos colocar o nome dele, em seguida os nomes das colunas juntamente com suas tipagens.
~~~SQL
1    =>CREATE TABLE ``nomeDaTabela´´(
2        ``nome´´ varchar(80), --cria uma coluna nome do tipo var char(80)
3        ``idade´´ int, --cria uma coluna idade com o tipo int (um numero inteiro)
4        ``dinheiro´´ real, --cria uma coluna dinheiro do tipo real( que se trata de um tipo fracionario com 6 casas de precisão)
5        ``data_de_nascimento´´ date --cria uma coluna data_de_nascimento com o tipo date
6        ``localizacao_atual´´ point -- cria uma coluna localizacao_atual com um tipo expecifico point   
7        );
~~~

Os espaços em branco (ou seja, espaços, tabulações e novas linhas) podem ser usados ​​livremente em comandos SQL. Isso significa que você pode digitar o comando alinhado de forma diferente do acima, ou mesmo tudo em uma linha. Dois travessões `( “--” )` introduzem `comentários`. O que quer que os siga é ignorado até o final da linha. `O SQL não faz distinção entre maiúsculas e minúsculas em relação a palavras-chave e identificadores`, **exceto** `quando os identificadores são colocados entre aspas para preservar o caso` (não feito acima).

OBS: O PostgreSQL pode ser personalizado com um número arbitrário de tipos de dados definidos pelo usuário. Conseqüentemente, `os nomes de tipo não são palavras-chave na sintaxe`, **exceto** quando necessário para oferecer suporte a `casos especiais no padrão SQL`. Char(N), varchar(N), date, time, timestamp e interval.

### **-Dropando uma tabela:**

~~~SQL
    =>DROP TABLE ``nomeDaTabela´´;
~~~

--------------------------------------------------

<div id="primary"></div>

## *-Criando uma propriedade primary key em uma tabela:*

~~~
    => CREATE TABLE pessoa
    (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100)
    );
~~~

--------------------------------------------------

<div id="fk"></div>

## *-Criando uma propriedade foreign key em uma tabela:*
Uma foreign key é um atributo que possui a função de gerar uma ligação entre tabelas através das primary keys, assim você pode acessar informações de outras tabelas por essa conexão.

~~~SQL
    => CREATE TABLE pessoa
    (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100),
        nomeDaTabelaNoSingular_id INT,
        CONSTRAINT fk_nomeDaTabelaReferenciadaNoSingular
        FOREIGN KEY (nomeDaTabelaReferenciadaNoSingular_id) REFERENCES nomeDaTabelaReferenciadaNoPlura(colunaDaPrimaryKey)
    );
~~~

OBS: O `CONSTRAINT` UTILIZADO NESSE CÓDIGO SE TRATA DE UMA FORMA DE SIMPLIFICAR OS SELECTS FUTURAMENTE.




--------------------------------------------------

<div id="idadostabela"></div>

## *-Inserindo dados em uma tabela:*
Para que você leitor possa acompanhar o processo, utilizarei aqui a tabela anteriormente criada (chamaremos ela de pessoa).

### **-Inserindo dados em uma tabela:**
~~~SQL
    =>INSERT INTO pessoa VALUES('Lucas', 20, 105.50, '01-10-1998', '(10.5, 192.1)');
~~~
As constantes que não são valores numéricos simples geralmente devem estar entre aspas simples ( ' ), como no exemplo

*Explicando:*
Esse comando irá inserir dentro da tabela "pessoa", os valores passados entre os (), onde eles seguem a ordem padrão difinida ao criar essa tabela.

**OBS:** CASO VOCÊ DESEJE INSERIR EM UMA TABELA E NÃO QUEIRA PASSAR O ID, BASTA COLOCAR `"VALUES(DEFAULT, ...)"`

**OBS:** CASO VOCÊ NÃO COLOQUE UM ELEMENTO PARA SER INSERIDO NA COLUNA ELE IRÁ VAZIO.

### **-Inserindo dados em uma tabela de maneira mais organizada:**
~~~SQL
    =>INSERT INTO pessoa (nome, idade, dinheiro, data_de_nascimento, localizacao_atual) VALUES ('Lucas', 20, 105.50, '01-10-1998', '(10.5, 192.1)');
~~~

Utilizando esse recurso você pode listar as colunas em uma ordem diferente se desejar ou até mesmo omitir algumas colunas (sendo assim incrementadas com valor vazio).

--------------------------------------------------

<div id="ctabelaparquivo"></div>

## *-Copiando uma tabela para um arquivo:*

~~~SQL
    =>COPY (SELECT * FROM ``nomeDaTabela´´) TO '``path´´'
~~~

Para ficar mais simples para o entendimento, vamos exemplificar:

~~~SQL
    =>COPY (SELECT * FROM pessoa) TO 'C:\Users\User\Desktop\arquivo.txt'
~~~

--------------------------------------------------

<div id="stabela"></div>

## *-Consultando uma Tabela:*
Para recuperar dados de uma tabela, a tabela é consultada. Uma instrução SQL `SELECT` é usada para fazer isso. A declaração dessa é dividida em `uma lista de seleção` (a parte que lista as colunas a serem retornadas), `uma lista de tabelas` (a parte que lista as tabelas das quais recuperar os dados) e `uma qualificação opcional` (a parte que especifica quaisquer restrições).

- Ao fazer essa consulta os dados da tabela X irão aparecer no console:
~~~SQL
    =>SELECT * FROM ``nomeDaTabela´´;
~~~
*Nesse select o "\*" tem significado de todas as colunas presentes na tabela X.*

- Ao fazermos uma consulta podemos selecionar colunas expecificas para aparecer no console:
~~~SQL
    =>SELECT nome, idade, dinheiro FROM pessoa;
~~~
*Resultado desse select:*

![img3](https://uploaddeimagens.com.br/images/003/177/012/full/img3.png?1617224105)

*Nesse caso a linha que tem o nome "Joao", não foi passado algumas colunas, por isso estão vazias.*

- Você pode escrever expressões, não apenas referências de coluna simples, na lista de seleção:
~~~SQL
    =>SELECT nome, idade*2 AS dobro, dinheiro FROM pessoa;
~~~
*Explicação:*
É possível ver esse select como sendo normal, porém existe uma expressão em "idade*2", que ao executarmos altera o valor recebido.
Alido a isso existe o comando `AS` que tem a função de nomear essa nova coluna que será gerada da expressão (sendo assim, ela é opcional).

*Resultado desse select:*

![img4](https://uploaddeimagens.com.br/images/003/177/038/full/img4.png?1617224877)

- Utilizando WHERE:

Uma consulta pode ser “ qualificada ” adicionando uma cláusula WHERE que especifica quais linhas são desejadas. Essa cláusula contém uma expressão booleana e apenas as linhas para as quais a expressão booleana é verdadeira são retornadas. Os operadores booleanos usuais ( AND, OR e NOT) são permitidos na qualificação.

~~~SQL
    =>SELECT nome, idade FROM pessoa WHERE idade > 10;
~~~
*Resultado desse select:*

![img5](https://uploaddeimagens.com.br/images/003/177/044/full/img5.png?1617225304)

- Ordenando SELECT (ORDER BY):

Você pode solicitar que os resultados de uma consulta sejam retornados em ordem classificada:

~~~SQL
    =>SELECT nome, idade FROM pessoa ORDER BY nome;
~~~
*Resultado desse select:*

![img6](https://uploaddeimagens.com.br/images/003/177/046/full/img6.png?1617225519)

OBS: Nesse sentido, caso você deseje ordernar levando mais de uma coluna em consideração, você pode utilizado a seguinte estrutura (...ORDER BY nome, idade, dinheiro;)(ao ordenar dessa maneira, será levado em consideração a ordem das colunas).

- Retirando elementos duplicados(DISTINCT):

Você pode solicitar que linhas duplicadas sejam removidas do resultado de uma consulta:

~~~SQL
    =>SELECT DISTINCT nome, idade FROM pessoa;
~~~

*Resultado desse select:*

![img7](https://uploaddeimagens.com.br/images/003/177/080/full/img7.png?1617227095)

OBS: Esse comando pode ser utilizado em conjunto com o ORDER BY.



--------------------------------------------------

<div id="join"></div>

## *-Associações entre tabelas (Consulta de junção):*
Até agora, nossas consultas acessaram apenas uma tabela por vez. Porém essas podem acessar várias tabelas de uma vez ou acessar a mesma tabela de forma que várias linhas da tabela sejam processadas ao mesmo tempo. Uma consulta que acessa várias linhas da mesma tabela ou de tabelas diferentes ao mesmo tempo é chamada de `consulta de junção`.

OBS: Para melhor entendimento dos exemplos que se seguem, entenda que crie duas tabelas / 1°- pessoa (nome varchar(200), idade int, dinheiro real, data_de_nascimento date, mora varchar(200), bairro varchar(200)) 2° - endereco (rua varchar(200), bairo varchar(200), cidade varchar(100), numero int)

Exemplo dessa consulta:
~~~SQL
    => SELECT * FROM pessoa, endereco WHERE pessoa.bairro = endereco.bairro;
~~~
OBS: Nesse exemplo utilizo o pessoa.bairro = endereco.bairro, pois ambas as tabelas tem colunas bairro com mesmo nome, assim é possível diferencia-las, porem com colunas distintas não a necessidade de utilizar essa sintaxe.

### **-Outra maneira de fazer essa consulta:**

~~~SQL
    => SELECT * FROM pessoa INNER JOIN endereco ON (pessoa.bairro = endereco.bairro);
~~~

### **-Junção externa a esquerda/direita:**

~~~SQL
    =>SELECT * FROM pessoa LEFT OUTER JOIN endereco ON (pessoa.bairro = endereco.bairro);
~~~

Esta consulta é chamada de junção externa a esquerda, porque a tabela mencionada à esquerda do operador de junção (JOIN) terá `em cada uma de suas linhas pelo menos uma aparição, enquanto a tabela à direita terá apenas as saídas de linhas que correspondem a alguma linha do tabela a esquerda`. Ao gerar uma linha da tabela esquerda para a qual não há correspondência da tabela direita, valores vazios (nulos) são substituídos pelas colunas da tabela direita.

OBS: caso você deseje inverter para a direita, basta substituir o LEFT por RIGHT.

### **-Apelidando tabelas:**
Para apelidarmos uma tabela, basta colocar seu apelido, após sua chamada no FROM:

~~~SQL
    => SELECT p.nome FROM pessoa p;
~~~

### **-Auto-junção:**
Isso ocorre quando fazemos um select de uma tabela com ela mesma.

~~~SQL
    => SELECT p1.nome as nome, p2.city as cidade FROM pessoa p1, pessoa p2;
~~~

--------------------------------------------------

<div id="agregacao"></div>

## *-Funções de agregação:*

Como a maioria dos outros produtos de banco de dados relacional, o PostgreSQL oferece suporte a funções agregadas. Uma função de agregação calcula um único resultado de várias linhas de entrada. Por exemplo, existem agregados para calcular a soma de todas linhas (sum), avg(média), max(máximo) e min(mínima) ao longo de um conjunto de linhas.

~~~SQL
    => SELECT max(idade) FROM pessoas;
~~~
*Irá mostrar a pessoa mais velha da tabela*

### **Funções de agregações:**
* `MAX()` = A função MAX analisa um conjunto de valores e retorna o maior entre eles.
* `MIN()` = MIN analisa um grupo de valores e retorna o menor entre eles.
* `SUM()` = A função SUM realiza a soma dos valores em uma única coluna e retorna esse resultado.
* `AVG()` = Com a função AVG podemos calcular a média aritmética dos valores em uma única coluna.
* `COUNT()` = A função COUNT retorna o total de linhas selecionadas.



### **-Utilizando essa funções agregadas em consultas com where:**

Os agregadores não podem ser usados no WHERE de maneira simples. Essa restrição existe pois a clausula WHERE determina quais linhas serão incluidas no calculo das agregações, sendo assim ela é chamada antes dos agregadores. Porém a consulta pode ser feita utilizando uma subconsulta:

~~~SQL
    => SELECT idade FROM pessoa WHERE idade = (SELECT max(idade) FROM pessoa);
~~~

###  **-Group by dentro do select:**
Um group by tem como função principal agrupar os elemento de acordo com uma coluna de sua tabela, sendo utilizada APENAS QUANDO HOUVER UMA FUNÇÃO AGREGADORA.

~~~SQL
    => SELECT count(name) from pessoa GROUP BY name;
~~~

OBS: Nesse caso se eu não colocar o group by toda a informação será baseada no todo, send assim terá como resultado uma informação geral.

### **-Having dentro do select:**
O having tem a função de criar uma condição para mostragem na tela.

~~~SQL
    => SELECT count(name) from pessoa HAVING count(name) < 5;
~~~
*Nesse caso o having ira mostrar na tela as informações que ao utilizarem a função agregadora count sejam menor que 5.*

OBS: APRESENTA AS MESMAS REGRAS DO GROUP BY / E PODE SER UTILIZADO EM CONJUNTO AO GROUP BY.

--------------------------------------------------

<div id="update"></div>

## *-Atualizações:*

### **-UPDATE:**
Você pode atualizar as linhas existentes usando o comando UPDATE.

~~~SQL
    => UPDATE pessoa SET name = 'OLA' WHERE pessoa.name = 'ola';
~~~

--------------------------------------------------

<div id="delete"></div>

## *-Deleções:*

### **-DELETE:**
Você pode excluir linhas de tabelas, utilizando o comando DELETE.

~~~SQL
    => DELETE FROM pessoa WHERE id = 1;
~~~

--------------------------------------------------

<div id="avancados"></div>

## **`-Conceitos um pouco mais avançados do SQL:`**

<div id="view"></div>

## *-VIEW:*
Suponha que a lista combinada de registros de certas tabelas seja de particular interesse para seu aplicativo, mas você não deseja digitar a consulta sempre que precisar. Você pode criar uma `view` sobre a consulta, que dá um nome à consulta ao qual você pode se referir como uma tabela comum.

~~~SQL
    => CREATE VIEW ``nomeDaView´´ AS
        SELECT name 
        FROM city, pessoa 
        WHERE pessoa.name = city.name;

    SELECT * FROM ``nomeDaView´´
~~~

As views permitem que você encapsule os detalhes da estrutura de suas tabelas, que podem mudar à medida que seu aplicativo evolui, por trás de interfaces consistentes.

As visualizações podem ser usadas em quase todos os lugares em que uma tabela real pode ser usada. Sendo assim, construir views sobre outras views não é incomum.

--------------------------------------------------

<div id="ofk"></div>

## *-Outra maneira de criar uma foreign key:*
Podemos utilizar uma maneira diferente para definir nossas FK, sendo essa: `<nomeDoCampo> <tipo> REFERENCES <tabelaRelacionada>(<campoRelacionado>)`

Exemplo:
~~~SQL
    CREATE TABLE cidades(
        id serial primary key,
        name varchar(100)
    );

    CREATE TABLE climas(
        id serial primary key,
        climate varchar(100),
        cidade_id int references cidades(id)
    );
~~~

--------------------------------------------------

<div id="transactions"></div>

## *-Transações (TRANSACTIONS):*
As transações possuem a função de rodar um código SQL somente se ele de certo por completo, se der algum erro ele não irá rodar. Para você fazer uma transaction é necessário adicionar ao inicio do seu código o termo `BEGIN;` e no final `COMMIT;`

Exemplo:
~~~SQL
    BEGIN;
    UPDATE people SET cpf = '11111'
    WHERE id = 23;
    COMMIT;
~~~

Caso você decida que não quer fazer as alterações é possível colocar a cláusula `ROLLBACK` em vez da COMMIT, assim cancelando todas as alterações.

Alido a isso é possível transformar a transaction em algo mais fracionado através da clausula `SAVEPOINT`, que salva todas as alterações vindas antes dela em um espaço especifico, assim podemos retornar a ele ou commita-lo.

Exemplo:
~~~SQL
    BEGIN;
    UPDATE people SET cpf = '11111'
    WHERE id = 23;
    SAVEPOINT update_people;
    UPDATE adresses SET city = 'ABC'
    WHERE adresses.id = 15;
    ROLLBACK TO update_people;
~~~

--------------------------------------------------

<div id="window"></div>

## *-Funções de janela (WINDOW FUNCTIONS):*
Uma função de janela executa um cálculo em um conjunto de linhas da tabela que estão de alguma forma relacionadas à linha atual. Isso é comparável ao tipo de cálculo que pode ser feito com uma função agregada. No entanto, as funções de janela não fazem com que as linhas sejam agrupadas em uma única linha de saída, como fariam as chamadas agregadas sem janela. Em vez disso, as linhas retêm suas identidades separadas.

Sintaxe:
~~~SQL
    SELECT <atributo1>, <atributo2>, funçãoagregadora(<atributo3>) OVER(PARTITION BY <atributo>) FROM <tabela>;
~~~

Exemplo:
~~~SQL
    SELECT name, age, count(age) OVER(PARTITION BY age) FROM people;
~~~


--------------------------------------------------

<div id="heranca"></div>

## *-Herança (INHERITS):*

Igualmente nas linguagens POO, o Sql aceita herança, onde uma tabela herda todas os atributos da outra:

Exemplo:
~~~SQL 
    CREATE TABLE city(
        name text,
        population int
    )

    CREATE TABLE capital(
        state varchar(2)
    ) INHERITS (city)
~~~

Após a criação dessas tabelas, caso você deseje acessar ambas tabelas um select simples irá resolver o problema, porém caso deseje retornar apenas a tabela principal, basta utilizar o ONLY

Exemplo:
~~~SQL
    SELECT * FROM ONLY city;
~~~

