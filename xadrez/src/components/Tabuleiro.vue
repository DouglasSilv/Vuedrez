<template>
    <div class="container">    
        <div class="tabuleiro" v-click-outside="limparQuadrados" v-on:click="limparQuadrados" >
            <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                <Quadrado 
                    :movimentos="movimentos" 
                    :mostraOpcoesPeao="mostraOpcoesPeao" 
                    :adicionaPeca="adicionaPeca" 
                    :adicionaQuadrado="adicionaQuadrado"
                    :removePecaAtual="removePecaAtual"
                    :isPecaSelecionada="isPecaSelecionada"
                    :eliminaPeca="eliminaPeca"
                    :linha="linha"
                    :coluna="coluna"
                    :pecaSelecionada="pecaSelecionada"
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
        getQuadrado(coluna, linha){
            coluna = String(coluna)
            linha = String(linha)
            const id = coluna+linha
            return this.quadrados.get(id)
        },
        adicionaPeca(linha, coluna, peca){
            this.pecas.push({linha, coluna, peca})
        },
        adicionaQuadrado(id, linha, coluna, ocupado, quadrado){
            this.quadrados.set(id, {linha, coluna, ocupado, quadrado})
        },
        mostraOpcoesPeao(peca, linha, coluna){
            var aux = peca.lado === 'Branco' ? aux = 1 : -1
            setTimeout(() => {
                const qntMovimentos = linha === 7 && peca.lado === 'Branco' || linha === 2 && peca.lado === 'Preto' ? 2 : 1
                var movimentos = []
  
                for (let index = 1; index <= qntMovimentos; index++) {
                    var mov = index*aux
                    if(this.getQuadrado(coluna, linha-mov).quadrado.__vue__.ocupado === true)
                        break
                    movimentos.push({id: ''+coluna+(linha-mov)})  
                }
  
                if(this.isOcupadoEValido(coluna+(1*aux), linha-(1*aux), peca.lado))
                    movimentos.push({id: String(coluna+(1*aux))+String(linha-(1*aux))})
                
                if(this.isOcupadoEValido(coluna-(1*aux), linha-(1*aux), peca.lado))
                    movimentos.push({id: String(coluna-(1*aux))+String(linha-(1*aux))})
  
                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1);
        },
        isOcupadoEValido(coluna, linha, lado){
            var adversario = ''
            if (lado === 'Branco'){
                adversario = 'Preto'
            }else {
                adversario = 'Branco'
            }

            if(this.getQuadrado(coluna, linha) 
               && this.getQuadrado(coluna, linha).quadrado.__vue__.ocupado === true 
               && this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado.lado === adversario)
                return true
            return false
        },
        getQuadrados(movimentos) {
            this.movimentos = []
            movimentos.forEach(movimento => {
                this.movimentos.push(String(movimento.id))
            })
        },
        limparQuadrados(){
            this.movimentos = []
            this.pecaSelecionada = {}
        },
        removePecaAtual(){
            this.getQuadrado(this.pecaSelecionada.coluna, this.pecaSelecionada.linha).quadrado.firstChild.remove()
            this.getQuadrado(this.pecaSelecionada.coluna, this.pecaSelecionada.linha).quadrado.__vue__.ocupado = false
            this.getQuadrado(this.pecaSelecionada.coluna, this.pecaSelecionada.linha).quadrado.__vue__.pecaQuadrado = {}
        },
        eliminaPeca(coluna, linha){
            this.getQuadrado(coluna, linha).quadrado.firstChild.remove() 
        },
        isPecaSelecionada(lado){
            return ((this.pecaSelecionada.hasOwnProperty('lado')) && lado != this.pecaSelecionada.lado)
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


