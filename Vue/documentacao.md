# **Documentação básica sobre Vue:**

## *Sumário:*
1.
2.
3.

## *Instalando o Vue:*

### **- Instalação:**

- Para instalar o Vue, basta entrar no site oficial e seguir os passos do "Getting Started".

- Caso você deseje pode também colar o seguinte código no seu html:
    ~~~js
        <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    ~~~

- **OBS:** Ele funciona até a data atual de 06/08/2021.

## *A instancia do Vue:*

- A instancia do Vue é o inicio para utilizar o framework, pois ao cria-la você passará um objeto(semelhante a um json), que terá os parametros, métodos entre outras coisas, que você vai poder usar no seu código.

- Exemplo:
    ~~~js 
        new Vue({
            el: "#app"
        })
    ~~~

### **- Elemento principal da sua aplicação(el):**

- O parametro **`el`** presente no objeto passado para a instancia Vue, é o elemento que você considera principal dentro da sua aplicação;

- Nele você pode passar uma string com o id do elemento;

- Exemplo:
    ~~~js
        <div id="app">
        </div>
        <script>
            new Vue({
                el: "#app"
            })
        </script>
    ~~~

### **- Dados que podem ser utilizados em seu el (data):**

- Com o parametro **`data`** sendo passado dentro do Vue, você pode definir "variaveis" que podem ser usadas em seu el (podem ser arrays, strings, booleans, int, float, função etc):

- Exemplo:
    ~~~js
        <div id="app">
            {{msg}}
        </div>
        <script>
            new Vue({
                el: "#app",
                data:
                {
                    msg: "Ola mundo",
                }
            })
        </script>
    ~~~

### **- Utilizando métodos dentro do seu el:**

- Outro parametro que pode ser passado dentro da sua instancia de Vue é o **`methods`**(dentro dele ficam as funções que você pode chamar dentro do seu el):

- Exemplo:
        ~~~js
        <div id="app">
            {{nomeDoMetodo()}}
        </div>
        <script>
            new Vue({
                el: "#app",
                data:
                {
                    msg: "Ola mundo",
                },
                methods:
                {
                    nomeDoMetodo() {
                        return "metodo: "+this.msg;
                    }
                }
            })
        </script>
    ~~~

### **- Diretivas do Framework:**

- As diretivas do framework tem a função de criar uma ligação melhor entre o Vue e o seu HTML:

    - `v-bind:` = é colocado antes de parametros HTML que você utilizará variaveis do data (OBS: caso você só coloque ":" também irá funcionar):

        - Exemplo:
            ~~~js
                <div id="app">
                    <input v-bind:placeholder=place>
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            place: "Ola mundo",
                        }
                    })
                </script>
            ~~~
        
    - `v-text=` = tem a função de escrever algo dentro de uma tag html, porém não entende as tags caso você coloque:

        - Exemplo:
            ~~~js
                <div id="app">
                    <p v-text=texto></p>
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            texto: "ola mundo"
                        }
                    })
                </script>
            ~~~

    - `v-html` = tem a mesma função que o v-text, porém compreende o html:
        - Exemplo:
            ~~~js
                <div id="app">
                    <p v-html=texto></p>
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            texto: "<strong>ola mundo</strong>"
                        }
                    })
                </script>
            ~~~

    - `v-model` = substitui o parametro value do html:
        - Exemplo:
            ~~~js
                <div id="app">
                    <input v-model = texto>
                    {{texto}}
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            texto: "ola mundo"
                        }
                    })
                </script>
            ~~~
        
    - `v-if = condicional / v-else-if = condicional / v-else` = é semelhante a um if comum das linguagens, porém fica dentro de um componente HMTL(Só irá mostrar o componente e seus filhos caso a condicional seja verdadeira)

        - Exemplo:
            ~~~js
                <div id = 'app'>
                    <input type="text" v-model=texto>
                    <div v-if= "texto == 'oi'">
                        <p>
                            oi!
                        </p>
                    </div>
                    <div v-else-if = "texto == 'ola'">
                        <p>
                            ola!
                        </p>
                    </div>
                    <div v-else>
                        <p>
                            Qualquer outra coisa!
                        </p>
                    </div>
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            texto: ""
                        }
                    })
                </script>
            ~~~
        
    - `v-for` = é como se fosse um foreach do python podendo ter "for(x in y)", porém dentro de um elemento HTML:
        
        - Exemplo:
            ~~~js
                <div id="app">
                    <input v-model = msg>
                    <button v-on:click = changeMsg>
                        Mudar para oi
                    </button>
                    <br/>
                    {{msg}}
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            "msg": ""
                        },
                        methods:
                        {
                            changeMsg()
                            {
                                this.msg = "oi";
                            }
                        }
                    })
                </script>
            ~~~

        - OBS: É possível colocar as chaves e indices adicionando essas variaveis antes do "in" no for.

    - `v-on:click` = funciona como o on click normal do JS, porém você pode alterar até mesmo as variaveis presente no seu código JS do vue:

        - Exemplo: 
            ~~~js
                <div id="app">
                    <input v-model = msg>
                    <button v-on:click = changeMsg>
                        Mudar para oi
                    </button>
                    <br/>
                    {{msg}}
                </div>
                <script>
                    new Vue({
                        el: "#app",
                        data:
                        {
                            "msg": ""
                        },
                        methods:
                        {
                            changeMsg()
                            {
                                this.msg = "oi";
                            }
                        }
                    })
                </script>
            ~~~

### **- Event e Prevent:**

- Com o VueJs é possível pegar o evento que está ocorrendo em determinadas ações, para isso basta colocar o parametro `$event` na função:

- Exemplo:
    ~~~js
        <div id="app">
             
            <input v-model = msg>
            <button v-on:click=changeMsg($event)>
                Mudar para oi
            </button>
            <br/>
            {{msg}}
            
        </div>
        <script>
            new Vue({
                el: "#app",
                data:
                {
                    "msg": "",
                    'test': ''
                },
                methods:
                {
                    changeMsg(event)
                    {
                        this.msg = "oi";
                        console.log(event);
                    }
                }
            })
        </script>
    ~~~

- Caso você pegue a informação do `event` você pode usar ela para aplicar o conceito de singlePageAplication, uma vez que com essa info você pode usar o método `preventeDefault()` que faz com que a requisição não seja enviada(assim não dando reload na pagina):

    - Exemplo:
        ~~~js
             <div id="app">
                <input v-model = msg>
                <button v-on:click=changeMsg($event)>
                    Mudar para oi
                </button>
                <br/>
                {{msg}}
                
            </div>
            <script>
                new Vue({
                    el: "#app",
                    data:
                    {
                        "msg": ""
                    },
                    methods:
                    {
                        changeMsg(event)
                        {
                            event.preventDefault();
                            this.msg = "oi";
                            
                        }
                    }
                })
            </script>
        ~~~
    
- Outra maneira de fazer esse processo é utilizando o `v-on:submit.prevent` no form:

    - Exemplo: 
        ~~~js
            <div id="app">
                <form v-on:submit.prevent=changeMsg()>
                    <input v-model = msg>
                    <button type="submit">
                        Mudar para oi
                    </button>
                    <br/>
                    {{msg}}
                </form>
                
            </div>
            <script>
                new Vue({
                    el: "#app",
                    data:
                    {
                        "msg": ""
                    },
                    methods:
                    {
                        changeMsg()
                        {   
                            this.msg = "oi";
                        }
                    }
                })
            </script>
        ~~~

### **-Binding Keyup:**

- Com essa funcionalidade você pode fazer com que ao clicar em determinada tecla uma função seja chamada(`v-on:keyup.<nomeDaTecla>`):

    - Exemplo:
        ~~~js
            <div id="app">
                <input v-model = msg v-on:keyup.o = changeMsg()>
                {{msg}}
            </div>
            <script>
                new Vue({
                    el: "#app",
                    data:
                    {
                        "msg": ""
                    },
                    methods:
                    {
                        changeMsg()
                        {   
                            this.msg = "oi";
                        }
                    }
                })
            </script>
        ~~~

### **- Manipulando elementos:**

- Para pegar um elemento especifico e altera-lo, você pode usar o `event.target.`:

    - Exemplo1:
        ~~~js
            <div id="app">
                <p v-on:click=changeColor($event)>
                    ola mundo!!
                </p>     
            </div>
            <script>
                new Vue({
                    el: "#app",
                    data:
                    {
                    },
                    methods:
                    {
                        changeColor(event)
                        {   
                            if(event.target.style.background == "blue none repeat scroll 0% 0%")
                            {
                                event.target.style = "background: white";
                            }
                            else
                            {
                                event.target.style = "background: blue";
                            }
                        }
                    }
                })
            </script>
        ~~~

    - OBS: Para pegar o estilo, colocamos .style, para pegar o conteudo colocamos .textContent.

### **- Manipulando elementos pela referencia:**

- Para fazer isso basta utilizar o `ref=<nomeDaReferencia>` dentro do seu elemento HTML e depois acessa-lo pelo `this.$refs.<nomeDaRef>.`:

- Exemplo:
    ~~~js
        <div id="app">
            <p ref = "paragraph" v-on:click=changeColor()>
                ola mundo!!
            </p>     
        </div>
        <script>
            new Vue({
                el: "#app",
                data:
                {
                },
                methods:
                {
                    changeColor()
                    {   
                        if(this.$refs.paragraph.style.background == "blue none repeat scroll 0% 0%")
                        {
                            event.target.style = "background: w";
                        }
                        else
                        {
                            event.target.style = "background: blue";
                        }
                    }
                }
            })
        </script>
    ~~~

### **- Instancias de classes Vue:**

- Você pode com o vue criar diversas instancias da classe Vue, assim criando ambientes de escopo distinto, até mesmo do escopo js.

### **- $mount:**

- Caso você não coloque o `el` você pode usar `.$mount("#<IdDoElQueQuerUsar>")`, assim ele irá montar a estrutura do VUE:

    - Exemplo:
        ~~~js
            <div id="app">
                <button onclick="a()">
                    botão
                </button>
                {{msg}}
            </div>
            <script>
                function a()
                {
                    app.$mount('#app');
                }
                const app = new Vue({
                    data:
                    {
                        msg: "oi"
                    }
                })
            </script>
        ~~~

### **- Components:**

- Utilizando o vue, é possível gerar componentes personalizados, bastando chamar `Vue.componente('<nome>', {}):

    - Exemplo:
        ~~~js
            <div id="app">
                <olamundo></olamundo>
            </div>
            <script>

                Vue.component('olamundo', 
                {
                    data()
                    {
                        return {
                            titulo: "ola mundo"
                        }
                    },
                    template: 
                    `
                    <div>
                        <h1>{{titulo}}</h1>
                        <button>teste</button>
                    </div>
                    `
                });
                
                const app = new Vue({
                    el: '#app'
                })
            </script>   
        ~~~

### **- Lifecycle:**

- https://www.youtube.com/watch?v=cX5YL4RZxD4&list=PLWNaqtzH6CWR-dykXeDD5XmMzJur9JBIh&index=21