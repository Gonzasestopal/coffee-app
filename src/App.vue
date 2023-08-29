<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/coffee.png" width="10%">
    <CoffeeForm/>
  </div>
</template>

<script>
import CoffeeForm from './components/CoffeeForm.vue'

export default {
  name: 'App',
  components: {
    CoffeeForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
