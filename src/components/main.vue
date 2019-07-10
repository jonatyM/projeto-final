<template>
    <div id="scta" class="table-secondary">
        <form >
            <div id="cadastroLivro" class="form-gruup ">
                
                <h2 align="center">{{ cabecalho }}</h2>
                <label><span class="red-alert">{{ vldLivro }}</span></label>
                <input v-model="novoLivro" type="text" placeholder="Titulo do livro..." class="form-control">
                <br>
                <label><span class="red-alert">{{ vldAutor }}</span></label>
                <input v-model="novoAutor" type="text" placeholder="Autor do livro..." class="form-control">
                <br/>
                <span class="input-group-btn">
                    <button @click="addLivro"  type="submit" v-bind:disabled="!isValid" class="btn btn-success btn-block">Adicionar</button>
                </span>
                <br>
            </div>

            <div id="tblLivro">
                <hr>
                <ul>
                    <li  class="toggle" v-for="livro of livros" :key="livro.titulo.autor"  v-bind:class="{ 'removido': livro.checked }">
                        <div class="checkbox">
                            <label>
                                <input type="checkbox" name="list" v-model="livro.checked">
                                <big>{{ livro.titulo }}</big> - <small>{{ livro.autor }}</small>
                                <span aligin="right" >
                                    <button class="btn btn-outline-danger" @click="removeLivro(livro)"> x </button>
                                </span>                            
                            </label>
                        </div>
                    </li>
                </ul>
            
            </div>

            <!--<footer class="footer" v-show="todos.length" v-cloak>
                <span class="todo-count">
                    <strong>{{ remaining }}</strong> {{ remaining | pluralize }} left
                </span>
                <ul class="filters">
                    <li><a href="#/all" :class="{ selected: visibility == 'all' }">Todos</a></li>
                    <li><a href="#/active" :class="{ selected: visibility == 'active' }">Ativados</a></li>
                    <li><a href="#/completed" :class="{ selected: visibility == 'completed' }">Concluído</a></li>
                </ul>
                <button class="clear-completed" @click="removeCompleted" v-show="livros.length > remaining">
                    Clear completed
                </button>   
            </footer> -->
          </form>
    </div>
</template>

<script>
export default {
    name: 'scta',
    data () {
        return {
            cabecalho: 'Livros Preferidos',
            vldLivro: "*",
            vldAutor: "*",            
            error: [],
            livros: [],
            novoLivro: "",
            novoAutor: "",                                  
        }
    },
    mounted() {
        if (localStorage.getItem('livros')) {
            try {
                this.livros = JSON.parse(localStorage.getItem('livros'));
            } catch(e) {
                localStorage.removeItem('livros');
            }
        }
    },
    computed:{
        isValid: function(){
            return this.novoLivro != "" && this.novoAutor != ""
            && this.novoLivro !== this.novoAutor
            //&& this.livros.novoLivro.length < 2 && this.livros.novoAutor.length < 4
        }
    },
    
    methods: {
        addLivro() {
            var titulo, autor,
            titulo = this.novoLivro.trim()
            autor = this.novoAutor.trim()            
            if (!titulo && autor) {
                return true
            }
            this.livros.push({titulo: titulo, autor: autor, checked: false })
            this.novoLivro = ""
            this.novoAutor = ""
            this.saveLivro();           
        }, 
        removeLivro(livro){
            this.livros.splice(this.livros.indexOf(livro), 1)
            this.saveLivro();
        },
        saveLivro() {
            const parsed = JSON.stringify(this.livros);
            localStorage.setItem('livros', parsed);
        },
    
       /*validaCampos() {
            var error = 0;
            var titulo, autor,
            titulo = this.novoLivro
            autor = this.novoAutor
            this.ResetError();
            if(this.livros.novoLivro.length < 4){
                this.error.vldLivro.push("Please, insert a valid name (4 characters)")
                error++;
            }

            if(this.livros.novoAutor.length < 4){
                this.error.vldAutor.push("Please, insert a valid name (4 characters)")
                error++;
            }

            /*if(this.contact.subject.length < 4){
                this.error.subject = "Invalid message (10 characters)";
                error++;
            }

            if(this.contact.message.length < 4){
                this.error.message = "Invalid message (10 characters)";
                error++;
            }/
            return (error === 0);
        },
        ResetError : function(){
            this.error.vldLivro = "*";
            this.error.vldAutor = "*";
        }, */       
    },
}
</script>
<style>

</style>
