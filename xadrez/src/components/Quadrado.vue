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
        adicionaPeca: Function
    },
    components: {
        Peca
    },
    methods:{
        getClass(){
            return {
                'branco': (this.linha+this.coluna)%2 == 0,  
                'preto': (this.linha+this.coluna)%2 != 0}
        }
    },
    mounted() {
        pecas.forEach(peca => {
            if(peca.linha === this.linha && peca.coluna === this.coluna){
                var ComponentClass = Vue.extend(Peca)
                var instance = new ComponentClass({
                    propsData: { tipo: peca.tipo,
                                 lado: peca.lado}
                })
                instance.$mount()
                this.$refs.quadrado.appendChild(instance.$el)
                console.log(this.$refs)
                this.adicionaPeca(this.linha, this.coluna, peca.tipo)
            }
        });
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

h1{
    color: orange
}
</style>
