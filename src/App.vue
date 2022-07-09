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
  background-color: #989898;
  background-image:
      linear-gradient(45deg, #666 25%, transparent 25%),
      linear-gradient(-45deg, #666 25%, transparent 25%),
      linear-gradient(45deg, transparent 75%, #666 75%),
      linear-gradient(-45deg, transparent 75%, #666 75%);
  background-size: 20px 20px;
  background-position: 0 0, 0 10px, 10px -10px, -10px 0;

}
</style>
