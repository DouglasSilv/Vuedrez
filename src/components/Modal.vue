<template>
    <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-header">
                <slot name="header">
                  Which piece do you want?
                </slot>
              </div>

              <div class="modal-body">
                <slot name="body">
                  <div v-bind:class="isSelecionada('Torre')" class="peca">
                    <img v-on:click="selecionarPeca('Torre')" :src="getImage('Torre')">
                  </div>
                  <div v-bind:class="isSelecionada('Rainha')" class="peca">
                    <img v-on:click="selecionarPeca('Rainha')" :src="getImage('Rainha')">
                  </div>
                  <div v-bind:class="isSelecionada('Bispo')" class="peca">
                    <img v-on:click="selecionarPeca('Bispo')" :src="getImage('Bispo')">
                  </div>
                  <div v-bind:class="isSelecionada('Cavalo')" class="peca">
                    <img v-on:click="selecionarPeca('Cavalo')" :src="getImage('Cavalo')">
                  </div>
                </slot>
              </div>

              <div class="modal-footer">
                <slot name="footer">
                  <button :disabled="isPecaSelecionada()" class="modal-default-button" v-on:click="evoluirPeca(pecaSelecionada, coluna, linha)">
                    OK
                  </button>
                </slot>
              </div>
            </div>
          </div>
        </div>
    </transition>
</template>

<script>
export default {
    name: "Modal",
    props: {
        lado: String,
        linha: Number,
        coluna: Number,
        evoluirPeca: Function
    },
    data: () => {
        return {
            pecaSelecionada: ''
        }
    },
    methods: {
        getImage(peca) {
            return require(`../assets/pecas/${peca+this.lado}.png`) 
        },
        selecionarPeca(peca){
            this.pecaSelecionada = peca
        },
        isSelecionada(peca){
            if(this.pecaSelecionada === peca) 
                return 'selecionada'
            else
                return 'nao-selecionada'
        },
        isPecaSelecionada(){
            if(this.pecaSelecionada === '') return true;
        }
    }
}
</script>

<style scoped>

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 55rem;
  margin: 0px auto;
  padding: 2rem 3rem;
  background-color: #fff;
  border-radius: .2rem;
  box-shadow: 0 .2rem .8rem rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: 'Viga', sans-serif;
  height: 22rem;
}

.modal-header {
  margin-top: 0;
  color: #42b983;
  font-size: 1.8rem;
}

.modal-body {
  margin: 2rem 0;
}

.modal-default-button {

}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.peca{
    display: inline-block;
    width: 9rem;
    height: 9rem;
    transition: all .2s;
    cursor: pointer;
}

.peca:hover{
    transform: scale(1.1);
}

.selecionada{
    border: #42b983;
    border-style: solid;
    border-width: 3px;
}

.nao-selecionada{
   border: transparent;
   border-style: solid;
   border-width: 3px; 
}

.peca:not(:last-child) {
    margin-right: 1.5rem;
}

</style>


