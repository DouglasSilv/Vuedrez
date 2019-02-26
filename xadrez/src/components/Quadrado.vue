<template>
    <div v-bind:class="getClass()" ref="quadrado" class="quadrado">
        {{linha}}-{{coluna}}
    </div>
</template>

<script>
import PeaoBranco from './PeaoBranco.vue'
import Vue from 'vue'

export default {
    name: "Quadrado",
    props: {
        linha: Number,
        coluna: Number,
        id: String
    },
    methods:{
        getClass(){
            return {
                'branco': (this.linha+this.coluna)%2 == 0,  
                'preto': (this.linha+this.coluna)%2 != 0}
        }
    },
    mounted() {
        var ComponentClass = Vue.extend(PeaoBranco)
        var instance = new ComponentClass()
        instance.$mount()
        this.$refs.quadrado.appendChild(instance.$el)
    }
}
</script>


<style scoped>
.quadrado {
    height: 8vw;
    width: 8vw;
    display: inline-block;
}

.preto{
    background-color: black;
}

.branco {
    background-color: white;
}
</style>
