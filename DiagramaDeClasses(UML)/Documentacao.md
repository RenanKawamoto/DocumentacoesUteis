# **Documentação Diagrama de classes (UML)**

## *O que é um diagrama de classe em UML?*

Antes de nós entendermos o que é um diagrama de classes, vamos entender o conceito de UML:

- UML: É uma linguagem de modelagem unificada, que facilita a padronização das documentações dos desenvolvedores.

Já o diagrama de classe é uma maneira grafica de representar as classes, com seus atributos, métodos e relações.

## *Componentes básicos de um diagrama de classes:*

- **Parte superior:** contém o nome da classe. Esta parte é sempre necessária, seja falando do classificador ou de um objeto.
- **Parte do meio:** contém os atributos da classe. Use esta parte para descrever as qualidades da classe. É necessário somente quando se descreve uma instância específica de uma classe.

    - Formatação:
        ~~~
            <ModificadorDeAcesso><nomeDoAtributo>:<tipoDoAtributo>
        ~~~

- **Parte inferior:** inclui as operações da classe (métodos). Exibido em formato de lista, cada operação ocupa sua própria linha. As operações descrevem como uma classe interage com dados.
    
    - Formatação:
        ~~~
            <ModificadorDeAcesso><nomeDoMétodo>(<Argumentos>:<tipoDoArgumento>):<tipoDeRetorno>
        ~~~


OBS: Classes abstratas são colocadas entre <<>>

## *Modificadores de acesso de membro:**
- Público (+)
- Privado (-)
- Protegido (#)
- Pacote (~)
- Derivado (/)
- Estático (sublinhado)

## *Interações:*

- SetaBranca/Vazia (Herança);
- LinhaSimples (Associação = Uma classe conhece os elementos da outra);
- LosangoVazio (Agregação = A parte pode existir fora do todo);
- LosangoPreenchido (Composição = Uma parte não pode existir sem a outra);