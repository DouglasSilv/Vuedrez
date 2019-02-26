<template>
    <div class="container">    
        <div class="tabuleiro">
            <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                <Quadrado :adicionaPeca="adicionaPeca" :linha="linha" :coluna="coluna" :id="''+coluna+linha" v-for="coluna in colunas" v-bind:key="coluna"/>
            </div>
        </div>
    </div>
</template>
<script>
import Quadrado from './Quadrado.vue'

export default {
  name: "Tabuleiro",
  components: {
      Quadrado
  },
  data: () => {
    return {
      linhas: [1, 2, 3, 4, 5, 6, 7, 8],
      colunas: [1, 2, 3, 4, 5, 6, 7, 8],
      pecas: []
    };
  },
  methods: {
      adicionaPeca(linha, coluna, peca){
          this.pecas.push({linha, coluna, peca})
      }
  }
}
</script>

<style scoped>
.tabuleiro {
    display: inline-block;
    border: 2px solid black;
    margin-left: 0 auto; 
}

.linha {
    height: 8vw;
}

.container{
    height: 100vh;
    text-align: center;
    margin-top: 5rem;
}
</style>


