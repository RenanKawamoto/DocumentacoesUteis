# **-Documentação de PostgreSQL**
--------------------------------------------------
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
    2. [Inserindo dados em uma tabela (INSET VALUE)](#idadostabela)
    3. [Copiando tabela para um arquivo](#ctabelaparquivo)
    4. [Consultando uma tabela, de maneira simples (SELECT)](#stabela)
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
    => 
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
    => 
~~~
--------------------------------------------------
--------------------------------------------------

<div id="sql"></div>

## **-A linguagem SQL:**
## *-Conceitos:*
PostgreSQL é um sistema de gerenciamento de banco de dados relacional ( RDBMS ). Isso significa que é um sistema de gerenciamento de dados armazenados nas relações. Relação é essencialmente um termo matemático para `tabelas (tables)`.

Cada tabela é uma coleção nomeada de `linhas`. Cada linha de uma determinada tabela possui o mesmo conjunto de `colunas` nomeadas e cada coluna possui um `tipo de dados específico`. Considerando que as colunas têm uma ordem fixa em cada linha, é importante lembrar que o SQL não garante a ordem das linhas na tabela de forma alguma (embora possam ser explicitamente classificadas para exibição).

As tabelas são agrupadas em `bancos de dados` e uma `coleção de bancos de dados gerenciados por uma única instância do servidor PostgreSQL` constitui um `cluster de banco de dados`.

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

## *-Associações entre tabelas (Consulta de junção):*
Até agora, nossas consultas acessaram apenas uma tabela por vez. Porém essas podem acessar várias tabelas de uma vez ou acessar a mesma tabela de forma que várias linhas da tabela sejam processadas ao mesmo tempo. Uma consulta que acessa várias linhas da mesma tabela ou de tabelas diferentes ao mesmo tempo é chamada de `consulta de junção`.

OBS: Para melhor entendimento dos exemplos que se seguem, entenda que crie duas tabelas / 1°- pessoa (nome varchar(200), idade int, dinheiro real, data_de_nascimento date, mora varchar(200), bairro varchar(200)) 2° - endereco (rua varchar(200), bairo varchar(200), cidade varchar(100), numero int)

Exemplo dessa consulta:
~~~SQL
    => SELECT * FROM pessoa, endereco WHERE pessoa.bairro = endereco.bairro;
~~~
OBS: Nesse exemplo utilizo o pessoa.bairro = endereco.bairro, pois ambas as tabelas tem colunas bairro com mesmo nome, assim é possível diferencia-las, porem com colunas distintas não a necessidade de utilizar essa sintaxe.


