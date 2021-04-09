# **Documentação SQL:**

## **Sumário:**
1.
2.
3.
4.

## **- Cap.1: Estrutura Lexical (Lexical Structure):**

`A entrada SQL consiste em uma sequência de comandos. Um comando é composto por uma sequência de tokens, terminados por um ponto e vírgula ( “ ; ” )`.O final do fluxo de entrada também encerra um comando. Quais tokens são válidos depende da sintaxe do comando específico.

Um token pode ser uma palavra-chave, um identificador, um identificador entre aspas, um literal (ou constante) ou um símbolo de caractere especial. Os tokens são normalmente separados por um espaço em branco (espaço, tabulação, nova linha), mas não precisam ser se não houver ambiguidade (o que geralmente é o caso apenas se um caractere especial for adjacente a algum outro tipo de token).

Além disso, `comentários podem ocorrer na entrada SQL. Eles não são tokens, eles são efetivamente equivalentes a espaços em branco.`

`A sintaxe SQL não é muito consistente em relação a quais tokens identificam comandos e quais são operandos ou parâmetros.`

-------------------------------------------

### **1.1: Identificadores e palavras-chave (Identifiers and Key Words):**

* Tokens como *SELECT, DELETE, DROP, VALUES, etc* são as chamadas **palavras-chave(key words)**.

* Os Tokens que nós definimos de alguma maneira, como por exemplo: *nome_da_minha_table, nome_do_meu_atributo, nome_do_meu_banco_de_dados, etc* são os conhecidos **identificadores(identifiers).** 

*OBS:  Como os identificadores dão os nomes para as tabelas, colunas ou outros objetos de banco de dados, dependendo do comando em que são usados. Às vezes são simplesmente chamados de “ nomes ”.*

Palavras-chave e identificadores têm a mesma estrutura lexical, o que significa que não se pode saber se um token é um identificador ou uma palavra-chave sem conhecer o idioma.

### <font color="#90FF90">*-Padrão para nomear um identificado:*</font>

**Identificadores SQL e palavras-chave devem começar com uma letra ( a - z) ou um sublinhado ( _ ). Os caracteres subsequentes em um identificador ou palavra-chave podem ser letras, sublinhados, dígitos ( 0 - 9)** ou cifrões ( $ ). Observe que **cifrões não são permitidos em identificadores de acordo com a letra do padrão SQL, portanto, seu uso pode tornar os aplicativos menos portáveis**. O padrão SQL não definirá uma palavra-chave que contenha dígitos ou comece ou termine com um sublinhado, portanto, os identificadores dessa forma estão protegidos contra possíveis conflitos com extensões futuras do padrão.

**Os identificadores tem um limite de tamanho de 63 bytes.**

**Palavras-chave e identificadores sem aspas não diferenciam maiúsculas de minúsculas.**

### <font color="#90FF90">*-Convenção:*</font>

**Uma convenção frequentemente usada é escrever palavras-chave em maiúsculas e nomes em minúsculas**

### <font color="#90FF90">*-Identificador delimitado/entre aspas:*</font>

Existe um segundo tipo de identificador: o **identificador delimitado ou identificador entre aspas**. Ele é formado por uma sequência arbitrária de caracteres entre aspas duplas ( " ). **Um identificador delimitado é sempre um identificador, nunca uma palavra-chave**. Portanto, "select" pode ser usado para se referir a uma coluna ou tabela chamada “select”.

Os identificadores entre aspas podem conter qualquer caractere, exceto o caractere com código zero.Isso permite construir nomes de tabelas ou colunas que de outra forma não seriam possíveis, como aqueles contendo espaços ou "e" comercial. A limitação de comprimento ainda se aplica.



