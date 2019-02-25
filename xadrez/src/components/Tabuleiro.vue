<template>
    <div class="tabuleiro">
        <div v-for="linha in linhas" v-bind:key="linha" class="linha">
            <Quadrado :linha="linha" :coluna="coluna" v-for="coluna in colunas" v-bind:key="coluna"/>
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
      colunas: [1, 2, 3, 4, 5, 6, 7, 8]
    };
  },
}
</script>

<style scoped>
.tabuleiro {
    width: 80rem;
    height: 80rem;
    background-color: orange;
    display: block;
}

.linha {
    height: 10rem;
}
</style>


