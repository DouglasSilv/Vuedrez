<template>
    <div class="container">    
        <div class="tabuleiro">
            <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                <Quadrado :linha="linha" :coluna="coluna" :id="''+coluna+linha" v-for="coluna in colunas" v-bind:key="coluna"/>
            </div>
        </div>
    </div>
</template>
<script>
import Quadrado from './Quadrado.vue'
import PeaoBranco from './PeaoBranco.vue'
import Pecas from '../assets/pecas.js'

export default {
  name: "Tabuleiro",
  components: {
      Quadrado
  },
  data: () => {
    return {
      linhas: [1, 2, 3, 4, 5, 6, 7, 8],
      colunas: [1, 2, 3, 4, 5, 6, 7, 8]
    };
  },
  mounted() {
      const pecas = new Pecas();
      console.log(pecas)
  }
}
</script>

<style scoped>
.tabuleiro {
    background-color: orange;
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


