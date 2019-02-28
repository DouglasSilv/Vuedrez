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
        removePecaAtual: Function,
        eliminaPeca: Function,
        mostraOpcoesCavalo: Function,
        isPecaSelecionada: Function,
        movimentos: Array,
        pecaSelecionada: Object
    },
    data: () => {
        return {
            ocupado: false,
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
                    this.mostraOpcoesCavalo(this.pecaQuadrado, this.linha, this.coluna)
                    break;
                default:
                    // code block
            }
        },
        moverPeca(){
            if(Object.entries(this.pecaSelecionada).length === 0 && this.pecaSelecionada.constructor === Object) return
            if(this.movimentos.includes(this.id)){
                this.removePecaAtual()
                if(this.ocupado)
                    this.eliminaPeca(this.coluna, this.linha)
                var ComponentClass = Vue.extend(Peca)
                let tipo = this.pecaSelecionada.tipo
                let lado = this.pecaSelecionada.lado
                var instance = new ComponentClass({
                    propsData: { tipo: tipo,
                                 lado: lado,
                                 mostraOpcoes: this.mostraOpcoes,
                                 isPecaSelecionada: this.isPecaSelecionada}
                })
                instance.$mount()
                this.$refs.quadrado.appendChild(instance.$el)
                this.pecaQuadrado = {tipo: this.pecaSelecionada.tipo,
                                     lado: this.pecaSelecionada.lado,
                                     linha: this.linha,
                                     coluna: this.coluna}
                this.ocupado = true
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
                                 mostraOpcoes: this.mostraOpcoes,
                                 isPecaSelecionada: this.isPecaSelecionada}
                })
                instance.$mount()
                this.$refs.quadrado.appendChild(instance.$el)
                this.adicionaPeca(this.linha, this.coluna, peca.tipo)
                this.adicionaQuadrado(this.id, this.linha, this.coluna, true, this.$refs.quadrado)
                this.ocupado = true
                this.pecaQuadrado = peca
            }
        });
        if(this.ocupado === false)
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
    transition: all .3s;
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
    position: absolute;
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
