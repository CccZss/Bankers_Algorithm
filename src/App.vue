<template>
  <div id="app">
    <home-page/>
  </div>
</template>

<script>
import homePage from './components/homePage'

export default {
  name: 'app',
  components: {
    homePage
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
  margin-top: 20px;
  min-width: 1800px;
}
</style>
