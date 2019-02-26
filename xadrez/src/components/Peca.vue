<template>
    <img v-on:click="selecionarPeca()" v-click-outside="deselecionarPeca" v-bind:class="isSelecionada()" class="peca" :src="getImage()" alt="Kiwi standing on oval">
</template>

<script>
import ClickOutside from 'vue-click-outside'

export default {
    name: 'Peca',
    props: {
        tipo: String,
        lado: String
    },
    data: () => {
        return {
            selecionada: false
        }
    },
    created() {
    },
    methods:{
        getImage(){
            return require(`../assets/pecas/${this.tipo}${this.lado}.png`)
        },
        selecionarPeca(){
            this.selecionada = !this.selecionada;
            switch(this.tipo) {
                case 'Peao':
                    mostraOpcoesPeao()
                    break;
                case 'Cavalo':
                    // code block
                    break;
                default:
                    // code block
}
        },
        isSelecionada(){
            if(this.selecionada === true){
                return 'selecionada';
            }
        },
        deselecionarPeca() {
            this.selecionada = false
        }
    },
    directives: {
        ClickOutside
    }
}
</script>

<style scoped>
.peca{
    height: 65%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    position: absolute;
    transition: all .2s;
}

.peca:hover{
    cursor: pointer;
}

.selecionada {
    height: 75%;
}
</style>

