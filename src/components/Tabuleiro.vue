<template>
    <div>
        <div class="pecas-mortas-pretas" > 
            <div v-for="peca in pecasMortasPretas" v-bind:key="peca.coluna + peca.linha+''"> <img class="peca" :src="getImage(peca)" alt=""> </div>
        </div>
        <div class="container"> 
            <h1>Lado atual: <span>{{ladoAtual}}</span></h1>  
            <div class="tabuleiro" v-click-outside="limparQuadrados" v-on:click="limparQuadrados" >
                <div v-for="linha in linhas" v-bind:key="linha" class="linha">
                    <Quadrado 
                        :movimentos="movimentos" 
                        :mostraOpcoesPeao="mostraOpcoesPeao" 
                        :mostraOpcoesCavalo="mostraOpcoesCavalo"
                        :mostraOpcoesBispo="mostraOpcoesBispo"
                        :mostraOpcoesTorre="mostraOpcoesTorre"
                        :mostraOpcoesRainha="mostraOpcoesRainha"
                        :mostraOpcoesRei="mostraOpcoesRei"
                        :adicionaPeca="adicionaPeca" 
                        :adicionaQuadrado="adicionaQuadrado"
                        :removePecaAtual="removePecaAtual"
                        :isPecaSelecionada="isPecaSelecionada"
                        :openModal="openModal"
                        :eliminaPeca="eliminaPeca"
                        :mudarLado="mudarLado"
                        :linha="linha"
                        :coluna="coluna"
                        :isLadoAtual="isLadoAtual"
                        :pecaSelecionada="pecaSelecionada"
                        :id="''+coluna+linha" 
                        v-for="coluna in colunas" 
                        v-bind:key="coluna"
                    />
                </div>
            </div>
             <Modal v-if="showModal" 
                    @close="showModal=false"
                    :linha="linhaModal"
                    :coluna="colunaModal"
                    :lado="ladoModal"
                    :evoluirPeca="evoluirPeca"/>
        </div>
    </div>
</template>
<script>
import Quadrado from './Quadrado.vue'
import ClickOutside from 'vue-click-outside'
import Modal from './Modal.vue'

export default {
    name: "Tabuleiro",
    components: {
        Quadrado, Modal
    },
    data: () => {
        return {
            linhas: [1, 2, 3, 4, 5, 6, 7, 8],
            colunas: [1, 2, 3, 4, 5, 6, 7, 8],
            pecas: [],
            quadrados: new Map(),
            movimentos: [],
            pecaSelecionada: {},
            ladoAtual: 'Branco',
            showModal: false,
            linhaModal: '',
            colunaModal: '',
            ladoModal: '',
            pecasMortasPretas: new Array()
        };
    },
    methods: {
        getImage(peca){
            return require(`../assets/pecas/${peca.tipo+peca.lado}.png`)
        },
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
  
                if(this.isOcupadoEValidoPeao(coluna+(1*aux), linha-(1*aux), peca.lado))
                    movimentos.push({id: String(coluna+(1*aux))+String(linha-(1*aux))})
                
                if(this.isOcupadoEValidoPeao(coluna-(1*aux), linha-(1*aux), peca.lado))
                    movimentos.push({id: String(coluna-(1*aux))+String(linha-(1*aux))})
  
                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1)
        },
        mostraOpcoesCavalo(peca, linha, coluna){
            setTimeout(() => {
                var movimentos = []

                if(this.isValido(coluna+1, linha-2, peca.lado))
                    movimentos.push({id: String(coluna+1)+String(linha-2)})
                
                if(this.isValido(coluna-1, linha-2, peca.lado))
                    movimentos.push({id: String(coluna-1)+String(linha-2)})

                if(this.isValido(coluna+1, linha+2, peca.lado))
                    movimentos.push({id: String(coluna+1)+String(linha+2)})

                if(this.isValido(coluna-1, linha+2, peca.lado))    
                    movimentos.push({id: String(coluna-1)+String(linha+2)})
                
                if(this.isValido(coluna-2, linha+1, peca.lado))    
                    movimentos.push({id: String(coluna-2)+String(linha+1)})

                if(this.isValido(coluna-2, linha-1, peca.lado))    
                    movimentos.push({id: String(coluna-2)+String(linha-1)})

                if(this.isValido(coluna+2, linha+1, peca.lado))    
                    movimentos.push({id: String(coluna+2)+String(linha+1)})

                if(this.isValido(coluna+2, linha-1, peca.lado))    
                    movimentos.push({id: String(coluna+2)+String(linha-1)})

                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1)
        },
        mostraOpcoesBispo(peca, linha, coluna) {
            setTimeout(() => {
                var movimentos = []

                movimentos = this.percorreDiagonal('+', '-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('+', '+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('-', '-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('-', '+', movimentos, coluna, linha, peca.lado)

                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1 )
        },
        mostraOpcoesTorre(peca, linha, coluna){
            setTimeout(() => {
                var movimentos = []

                movimentos = this.percorreHorizontal('-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreHorizontal('+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreVertical('+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreVertical('-', movimentos, coluna, linha, peca.lado)
                
                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1)
        },
        mostraOpcoesRainha(peca, linha, coluna){
            setTimeout(() => {
                var movimentos = []

                movimentos = this.percorreHorizontal('-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreHorizontal('+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreVertical('+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreVertical('-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('+', '-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('+', '+', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('-', '-', movimentos, coluna, linha, peca.lado)
                movimentos = this.percorreDiagonal('-', '+', movimentos, coluna, linha, peca.lado)

                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1)
        },
        mostraOpcoesRei(peca, linha, coluna){
            setTimeout(() => {
                var movimentos = []

                for (let i = -1; i <= 1; i++) {
                    for (let j = -1; j <= 1; j++) {
                        if(this.isValido(coluna+i, linha+j, peca.lado))
                            movimentos.push({id: String(coluna+i)+String(linha+j)})
                    }
                    
                }

                this.getQuadrados(movimentos)
                this.pecaSelecionada = peca
            }, 1)
        },
        percorreDiagonal(sinal1, sinal2, movimentos, coluna, linha, lado){
            sinal1 = this.getSinal(sinal1)
            sinal2 = this.getSinal(sinal2)
            for (let i = 1; true; i++) {
                if(!this.isValido(coluna+(i*sinal1), linha+(i*sinal2), lado)) break

                movimentos.push({id: String(coluna+(i*sinal1))+String(linha+(i*sinal2))})
                
                if (this.getQuadrado((coluna+(i*sinal1)), (linha+(i*sinal2))) 
                && this.getQuadrado((coluna+(i*sinal1)), (linha+(i*sinal2))).quadrado.__vue__.pecaQuadrado.lado == this.getAdversario(lado)) break
            }

            return movimentos
        },
        percorreHorizontal(sinal, movimentos, coluna, linha, lado){
            sinal = this.getSinal(sinal)

            for (let i = 1; true; i++) {
                if(!this.isValido(coluna+(i*sinal), linha, lado)) break
                movimentos.push({id: String(coluna+(i*sinal))+String(linha)}) 
                
                if(this.getQuadrado(coluna+(i*sinal), linha) 
                && this.getQuadrado(coluna+(i*sinal), linha).quadrado.__vue__.pecaQuadrado.lado == this.getAdversario(lado))
                    break;
            }

            return movimentos
        },
        percorreVertical(sinal, movimentos, coluna, linha, lado){
            sinal = this.getSinal(sinal)

            for (let i = 1; true; i++) {
                if(!this.isValido(coluna, linha+(i*sinal), lado)) break
                movimentos.push({id: String(coluna)+String(linha+(i*sinal))}) 
                
                if(this.getQuadrado(coluna, linha+(i*sinal)) 
                && this.getQuadrado(coluna, linha+(i*sinal)).quadrado.__vue__.pecaQuadrado.lado == this.getAdversario(lado))
                    break;
            }

            return movimentos
        },
        getSinal(sinal){
            if(sinal === '+'){
                return 1
            } else{
                return -1
            }
        },
        getAdversario(lado) {
            var adversario = ''
            if (lado === 'Branco'){
                return adversario = 'Preto'
            }else {
                return adversario = 'Branco'
            }
        },
        isOcupadoEValidoPeao(coluna, linha, lado){
            var adversario = this.getAdversario(lado)

            if(this.getQuadrado(coluna, linha) 
               && this.getQuadrado(coluna, linha).quadrado.__vue__.ocupado === true 
               && this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado.lado === adversario)
                return true
            return false
        },
        isValido(coluna, linha, lado){
            if(this.getQuadrado(coluna, linha) && this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado.lado != lado)
                return true
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
            if(this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado.lado === 'Preto'){
                this.pecasMortasPretas.push(this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado)
                console.log('morreu');
            }
            this.getQuadrado(coluna, linha).quadrado.firstChild.remove() 
        },
        isPecaSelecionada(lado){
            return ((this.pecaSelecionada.hasOwnProperty('lado')) && lado != this.pecaSelecionada.lado)
        },
        mudarLado(){
            this.ladoAtual = this.getAdversario(this.ladoAtual)
        },
        isLadoAtual(lado){
            return this.ladoAtual === lado ? false : true
        },
        openModal(coluna, linha, lado){
            this.showModal = true;
            this.colunaModal = coluna;
            this.linhaModal = linha;
            this.ladoModal = lado
        },
        evoluirPeca(peca, coluna, linha){
            this.showModal = false
            this.getQuadrado(coluna, linha).quadrado.__vue__.pecaQuadrado.tipo = peca
            this.getQuadrado(coluna, linha).quadrado.firstChild.__vue__.tipo = peca
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

.container h1{
    font-family: 'Viga', sans-serif;
}

h1 span{
    color: #42b983;
}

@media (max-width: 768px) {
    .linha {
        height: 8vw;
    }
}

.pecas-mortas-pretas{
    top: 8rem;
    left: 4rem;
    position: absolute;
    max-height: 100rem;
    width: 16rem;
    list-style: none;
    background-color: blue;
    display: -webkit-inline-box;
}

.peca{
    height: 5rem;
    width: auto;
}
</style>


