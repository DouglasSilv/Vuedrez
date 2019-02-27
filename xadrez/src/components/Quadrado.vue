<template>
    <div v-bind:class="getClass()" ref="quadrado" class="quadrado">
        <h1>{{linha}} - {{coluna}}</h1>
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
        mostraOpcoesPeao: Function
    },
    data: () => {
        return {
            disponivel: false
        }
    },
    components: {
        Peca
    },
    methods:{
        getClass(){
            return {
                'branco': (this.linha+this.coluna)%2 == 0,  
                'preto': (this.linha+this.coluna)%2 != 0}
        },
        mostraOpcoes: function (tipo) {
            console.log('passou')
            switch(tipo) {
                case 'Peao':
                    mostraOpcoesPeao(this.linha, this.coluna)
                    break;
                case 'Cavalo':
                    // code block
                    break;
                default:
                    // code block
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
                console.log(this.$refs)
                this.adicionaPeca(this.linha, this.coluna, peca.tipo)
                this.adicionaQuadrado(this.linha, this.coluna, true)
                this.disponivel = true
            }
        });
        if(this.disponivel === false)
            this.adicionaQuadrado(this.linha, this.coluna, false)
    }
}
</script>


<style scoped>
.quadrado {
    height: 8vw;
    width: 8vw;
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
</style>
