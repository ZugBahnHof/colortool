<template>
  <div id="color-window" :style="{backgroundColor: color}">
    rgba({{ red }}, {{ green }}, {{ blue }}, {{ alpha }})
    <color-slider @input="(e) => {sliderInput(e, 'red')}"  v-model="red"></color-slider>
    <color-slider @input="(e) => {sliderInput(e, 'green')}"  v-model="green"></color-slider>
    <color-slider @input="(e) => {sliderInput(e, 'blue')}"  v-model="blue"></color-slider>
    <color-slider @input="(e) => {sliderInput(e, 'alpha')}"  v-model="alpha" is-alpha></color-slider>
    <input type="text" @input="transform" v-model="colorString">
  </div>
</template>

<script>
import ColorSlider from "@/components/ColorSlider";
const Color = require("color");
const ColorString = require('color-string');

export default {
  name: 'ColorWindow',
  components: {ColorSlider},
  props: {
    msg: String
  },
  data() {
    let tempColor = Color("#da1f3d");
    return {
      color: tempColor,
      colorString: tempColor.hexa().toString(),
      red: tempColor.red(),
      green: tempColor.green(),
      blue: tempColor.green(),
      alpha: tempColor.alpha(),
    }
  },
  computed: {
    hump() {
      if (this.color.valpha === 1) return this.color.hex().toString();
      return this.color.hexa().toString();
    }
  },
  methods: {
    transform(event) {
      let c = ColorString.get(event.target.value);
      if (c !== null) {
        this.color = Color(event.target.value);
      }
      // let str = event.target.value.toUpperCase();
      // str = str.replace(/[^A-F0-9]+/gi, "")
      // str = str.substr(0, 9);
      // str = "#" + str;
      // this.colorString = str;
    },
    randomColor() {
      return ""
    },
    sliderInput(event, color) {
      let value = event.target.value;

      switch (color) {
        case "alpha":
          if (0 <= value <= 1) this.color = this.color.alpha(value);
          break;

        case "red":
          if (0 <= value <= 255) this.color = this.color.red(value);
          break;

        case "green":
          if (0 <= value <= 255) this.color = this.color.green(value);
          break;

        case "blue":
          if (0 <= value <= 255) this.color = this.color.blue(value);
          break;
      }

      this.colorString = this.hump;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#color-window {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column-reverse;
  align-items: flex-start;
}

input[type=text] {
  font-size: 15vmin;
  line-height: 0.9;
  margin: 3rem;
  background-color: transparent;
  border: none;
}
input:focus {
  outline: none;
}
</style>
