# **Documentação básica sobre o NPM**

## *Sumário:*
1.
2.
3.

## *O que é o NPM?*

- O NPM se trata de um gerenciador de pacotes, fornecido pelo nodejs. Com ele é possível instalar bibliotecas, frameworks e recursos para o js.

## *Configurando sua conta npm:*

### **- Criando uma nova conta no registro publico:**

- Caso você não tenha uma conta npm, você pode cria-la para baixar e compartilhar pacotes js de maneira pública.

- Para cria-la basta entrar no site oficial e clicar em "Sing In", preencher os campos, aceitar os termos e clicar em "Create an account".

> Caso você deseja testar se sua conta foi cadastrada com sucesso basta usar o comando em seu terminal `npm login`.

> Para ver qual usuário você está usando basta utilizar o comando `npm whoami`.

## *Gerenciando sua conta npm:*

### **- Gerenciando seu perfil:**

- Existem duas maneiras de configurar seu perfil: através do site e através de linha de comando, como a primeira opção é bem simples e autoexplicativa, irei mostrar aqui somente a segunda.

- Para ver todas as configurações de seu perfil basta utilizar o seguinte comando:
    ~~~npm
    npm profile get 
    ~~~

- Existem alguns campos que podem ser alterados pela CLI(linha de comando), são eles:
    - email
    - two-factor auth
    - fullname
    - homepage
    - freenode
    - twitter
    - github
    - password

- Para alterar