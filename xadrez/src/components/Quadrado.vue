<template>
    <div v-bind:class="getClass()" class="quadrado">
    </div>
</template>

<script>
export default {
    name: "Quadrado",
    props: {
        linha: Number,
        coluna: Number
    },
    methods:{
        getClass(){
            return {
                'branco': (this.linha+this.coluna)%2 == 0,  
                'preto': (this.linha+this.coluna)%2 != 0}
    }
}
}
</script>


<style scoped>
.quadrado {
    height: 10rem;
    width: 10rem;
    display: inline-block;
    color: white;
    text-align: center;
    font-size: 3rem;
}

.preto{
    background-color: black;
}

.branco {
    background-color: white;
}
</style>
