<template>
  <div id="app">
    <Tabuleiro/>
  </div>
</template>

<script>
import Tabuleiro from './components/Tabuleiro.vue'

export default {
  name: 'app',
  components: {
    Tabuleiro
  }
}
</script>

<style>
*,
*::after,
*::before {
  margin: 0;
  padding: 0;
  box-sizing: inherit; }

html {
  font-size: 62.5%; }

body {
  box-sizing: border-box; }

</style>
