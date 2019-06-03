<template>
  <main id="app">
    <DisplayColors />
  </main>
</template>

<script>
import DisplayColors from './components/DisplayColors.vue'

export default {
  name: 'app',
  components: {
    DisplayColors
  }
}
</script>

<style>
body {
  background-color: rgb(233, 227, 227);
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
  margin-top: 60px;
}

section {
  width: 60vw;
  margin: 60px auto;
}
</style>
