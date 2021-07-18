<template>
  <div id="app">
    <CalculatorOne />
  </div>
</template>

<script>
import CalculatorOne from '@/components/CalculatorOne'

export default {
  name: 'App',
  components: {
    CalculatorOne
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
