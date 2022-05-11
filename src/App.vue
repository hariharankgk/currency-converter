<template>
  <div id="app">
    <ConverterBlock/>
  </div>
</template>

<script>
import ConverterBlock from './components/Converter.vue'

export default {
  name: 'App',
  components: {
    ConverterBlock
  }
}
</script>

<style lang="scss">
*, ::after, ::before {
    box-sizing: border-box;
}
body {
  margin: 0;
  padding: 0;
  background-color: #eee;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; 
}
</style>
