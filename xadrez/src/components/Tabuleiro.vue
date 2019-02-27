<template>
    <div class="container">    
        <div class="tabuleiro">
            <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                <Quadrado :mostraOpcoesPeao="mostraOpcoesPeao" :adicionaPeca="adicionaPeca" :adicionaQuadrado="adicionaQuadrado" :linha="linha" :coluna="coluna" :id="''+coluna+linha" v-for="coluna in colunas" v-bind:key="coluna"/>
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
      pecas: [],
      quadrados: []
    };
  },
  methods: {
      adicionaPeca(linha, coluna, peca){
          this.pecas.push({linha, coluna, peca})
      },
      adicionaQuadrado(linha, coluna, disponivel){
          this.quadrados.push({linha, coluna, disponivel})
      },
      mostraOpcoesPeao(linha, coluna){
          console.log('passou')
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
    height: 10vh;
}

.container{
    text-align: center;
    margin-top: 5rem;
}
</style>


