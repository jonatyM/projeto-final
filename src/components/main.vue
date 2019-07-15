<template>
    <div id="scta">
        <div class="img">
        </div>
        <div id="cadastroLivro" class="table-secondary">            
            <form @submit.prevent="addLivro" class="form-gruup ">               
                <h2>{{ cabecalho }}</h2>               
                <label>
                    <span class="red-alert" v-if="!novoLivro && !novoAutor">{{ vldCampos1 }}</span>
                    <span class="red-alert" v-if="novoLivro == novoAutor">{{ vldCampos2 }}</span>
                    <br>
                    <span class="red-alert" v-if="novoLivro.length < 2">{{ vldCampos3 }}</span>
                    <br>
                    <span class="red-alert" v-if="novoAutor.length < 4">{{ vldCampos4 }}</span>
                </label>
                <br>
                <input v-model="novoLivro" type="text" placeholder="Titulo do livro..." class="form-control">
                <br>
                <input v-model="novoAutor" placeholder="Autor do livro..." class="form-control">
                <br/>
                <span class="input-group-btn">
                    <button @click="addLivro"  type="submit" v-bind:disabled="!isValid" class="btn btn-success btn-block">Adicionar</button>
                </span>
                <br>            
            </form>
        </div>

        <div id="tblLivro" >
            <hr>
            <section v-for="livro in livros" :key="livro.titulo.autor"  v-bind:class="{ 'removido': livro.checked }">
                <input type="checkbox" name="list" v-model="livro.checked">
                <label>{{ livro.titulo }} - {{ livro.autor }}</label>
                <span>
                    <button id="bo" class="btn btn-outline-danger" @click="removeLivro(livro)">x</button>
                </span>
            </section>               
        </div>                 
    </div>
</template>

<script>
export default {
    name: 'scta',    
    data () {
        return {
            cabecalho: 'Livros Preferidos',
            vldCampos1: "Campos obrigatorios.",
            vldCampos2: "Não podem ser igual",
            vldCampos3: "Campo titulo do livro deve ter no minimo 2 caracteres.",
            vldCampos4: "Campo Autor do livro deve ter no minimo 4 caracteres.",
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
            && this.novoLivro.length >= 2 && this.novoAutor.length >= 4;
        },        
    },
    methods: {
        addLivro() {
            var titulo, autor;
            titulo = this.novoLivro.trim();
            autor = this.novoAutor.trim();            
            if (!titulo && autor) {
                return true;
            }
            this.livros.push({titulo: titulo, autor: autor, checked: false });
            this.novoLivro = "";
            this.novoAutor = "";
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
    },
}
</script>