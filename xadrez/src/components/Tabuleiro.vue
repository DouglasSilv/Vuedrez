<template>
    <div class="container">    
        <div class="tabuleiro" v-click-outside="limparQuadrados" v-on:click="limparQuadrados" >
            <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                <Quadrado 
                    :movimentos="movimentos" 
                    :mostraOpcoesPeao="mostraOpcoesPeao" 
                    :adicionaPeca="adicionaPeca" 
                    :adicionaQuadrado="adicionaQuadrado" 
                    :linha="linha" 
                    :coluna="coluna"
                    :peca="pecaSelecionada"
                    :id="''+coluna+linha" 
                    v-for="coluna in colunas" 
                    v-bind:key="coluna"
                />
            </div>
        </div>
    </div>
</template>
<script>
import Quadrado from './Quadrado.vue'
import ClickOutside from 'vue-click-outside'

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
        quadrados: new Map(),
        movimentos: [],
        pecaSelecionada: {}
      };
  },
  methods: {
      adicionaPeca(linha, coluna, peca){
          this.pecas.push({linha, coluna, peca})
      },
      adicionaQuadrado(id, linha, coluna, disponivel, quadrado){
          this.quadrados.set(id, {linha, coluna, disponivel, quadrado})
      },
      mostraOpcoesPeao(peca, linha, coluna){
          setTimeout(() => {
              const qntMovimentos = linha === 7 ? 2 : 1
              var movimentos = []

              for (let index = 1; index <= qntMovimentos; index++) {
                  movimentos.push({id: ''+coluna+linha-index, linha: linha-index , coluna})  
              }

              this.getQuadrados(movimentos)
              this.pecaSelecionada = peca
          }, 1);
      },
      getQuadrados(movimentos) {
          this.movimentos = []
          movimentos.forEach(movimento => {
              this.movimentos.push(String(movimento.id))
          })
      },
      limparQuadrados(){
          this.movimentos = []
      }
    },
    directives: {
        ClickOutside
    }
}
</script>

<style scoped>
.tabuleiro {
    display: inline-block;
    border: 2px solid black;
    margin: 0 auto; 
}

.linha {
    height: 10vh;
}

.container{
    text-align: center;
    margin-top: 5rem;
}

@media (max-width: 768px) {
    .linha {
        height: 8vw;
    }
}
</style>


