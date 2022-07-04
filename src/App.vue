<template>
<!--  <img alt="Vue logo" src="./assets/logo.png">-->
  <ColorWindow msg="Welcome to Your Vue.js App"/>
</template>

<script>
import ColorWindow from './components/ColorWindow.vue'

export default {
  name: 'App',
  components: {
    ColorWindow
  },
}
</script>

<style>
@import "../node_modules/normalizecss/normalize.css";
#app {
  font-family: Roboto Mono, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  min-height: 100vh;
  margin: 0;
  padding: 0;
}
</style>
