<template>
  <stepCom msg="Welcome to Your Vue.js App"/>
</template>

<script>
import stepCom from './components/stepCom.vue'

export default {
  name: 'App',
  components: {
    stepCom
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
