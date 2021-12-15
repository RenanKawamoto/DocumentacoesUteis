# **PHPUnit com Laravel:**

## *Sumário:*
1.
2.
3.
4.
5.

## *Primeiros passos:*

### **Introdução:**

- O Laravel já foi pensando para suportar testes utilizando o PHPUnit, sendo assim ele nos disponibiliza um arquivo chamado phpunit.xml (que já vem em nossa aplicação por padrão), que já está pré configurado.

- Ao criar um projeto o Laravel irá disponibilizar um diretorio chamado de `tests` e dentro deste existem dois diretórios: `Features` e `Unit`:
    
    - Os testes presente dentro do diretorio Unit se concentram em uma parte muito pequena e isolada do seu código(costumam se concentrar em um único método).

    - Já os testes dentro do dirtorio `Features` podem testar uma parte maior do seu código, incluindo como vários objetos interagem entre si ou até mesmo uma solicitação HTTP completa.

> OBS: testes dentro do diretorio `Unit` não inicializam sua aplicação, sendo assim, não possuem acesso ao banco de dados ou outros serviços do framework.


### **Ambiente:**

- Ao rodar um teste o Laravel automaticamente irá setar um "ambiente de configuração/configuration environment" para testes por conta das variaveis definidas no arquivo `phpunit.xml`. Alem disso o Laravel configura a sessão e o cache para o driver `array` durante os testes, o que significa que nenhum dados da sessão ou do cache será persistido durante seus testes.



