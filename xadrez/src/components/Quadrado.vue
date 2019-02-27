<template>
    <div v-bind:class="getClass()" v-on:click="moverPeca" ref="quadrado" class="quadrado">
    </div>
</template>

<script>
import Peca from './Peca.vue'
import pecas from '../assets/pecas.js'
import Vue from 'vue'

export default {
    name: "Quadrado",
    props: {
        linha: Number,
        coluna: Number,
        id: String,
        adicionaPeca: Function,
        adicionaQuadrado: Function,
        mostraOpcoesPeao: Function,
        limparQuadrados: Function,
        movimentos: Array,
        peca: Object
    },
    data: () => {
        return {
            disponivel: false,
            pecaQuadrado: {}
        }
    },
    components: {
        Peca
    },
    methods:{
        getClass(){
            var classe = (this.linha+this.coluna)%2 == 0 ? 'branco' : 'preto' 
            classe+= this.movimentos.includes(this.id) ? ' selecionado' : ''
            return classe
        },
        mostraOpcoes: function (tipo) {
            switch(tipo) {
                case 'Peao':
                    this.mostraOpcoesPeao(this.pecaQuadrado, this.linha, this.coluna)
                    break;
                case 'Cavalo':
                    // code block
                    break;
                default:
                    // code block
            }
        },
        moverPeca(){
            if(this.movimentos.includes(this.id)){
                var ComponentClass = Vue.extend(Peca)
                var instance = new ComponentClass({
                    propsData: { tipo: this.peca.tipo,
                                 lado: this.peca.lado,
                                 mostraOpcoes: this.mostraOpcoes}
                })
                instance.$mount()
                this.$refs.quadrado.appendChild(instance.$el)
            }
        }
    },
    mounted() {
        pecas.forEach(peca => {
            if(peca.linha === this.linha && peca.coluna === this.coluna){
                var ComponentClass = Vue.extend(Peca)
                var instance = new ComponentClass({
                    propsData: { tipo: peca.tipo,
                                 lado: peca.lado,
                                 mostraOpcoes: this.mostraOpcoes}
                })
                instance.$mount()
                this.$refs.quadrado.appendChild(instance.$el)
                this.adicionaPeca(this.linha, this.coluna, peca.tipo)
                this.adicionaQuadrado(this.id, this.linha, this.coluna, true, this.$refs.quadrado)
                this.disponivel = true
                this.pecaQuadrado = peca
            }
        });
        if(this.disponivel === false)
            this.adicionaQuadrado(this.id, this.linha, this.coluna, false, this.$refs.quadrado)
    }
}
</script>


<style scoped>
.quadrado {
    height: 10vh;
    width: 10vh;
    display: inline-block;
    position: relative;
}

.preto{
    background-color: black;
}

.branco {
    background-color: white;
}

.quadrado h1{
    color: orange;
    font-size: 4rem;
    margin-top: 3rem;
}

.selecionado {
    background-color: blue;
    cursor: pointer;
}

@media (max-width: 768px) {
    .quadrado {
        height: 8vw;
        width: 8vw;
    }
}
</style>
