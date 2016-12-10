<template>
  <div id="app">
    <img src="./assets/logo.png">
    <input-view></input-view>
  </div>
</template>

<script>
import inputView from './components/inputView'

export default {
  name: 'app',
  components: {
    inputView
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
