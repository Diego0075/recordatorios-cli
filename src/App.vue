<template>
  <div id="app">
    <Cabecera/>
    <Notas/>
  </div>
</template>

<script>

import Cabecera from './components/cabecera.vue'
import Notas from './components/notas.vue'

export default {
  name: 'App',
  components: {
    Cabecera,
    Notas
      }
}
</script>

<style>
 
</style>
