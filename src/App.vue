<template>
  <div id="app">
    <logo></logo>
  </div>
</template>

<script>
import logo from './components/logo.vue';
export default {
  components: {
    logo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
