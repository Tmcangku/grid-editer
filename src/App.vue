<template>
  <div id="app">
    <GridEditer propData="Welcome to Your Vue.js App"></GridEditer>
  </div>
</template>
<script>
import GridEditer from './components/GridEditer.vue';

export default {
  name: 'App',
  components: {
    GridEditer,
  }
}
</script>

<style>
#app {
width: 100vw;
height: 100vh;
background-color: blanchedalmond;
}
</style>