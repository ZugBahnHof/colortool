<template>
  <div id="color-window" :style="{backgroundColor: color, color: textColor}">

    back: rgba({{ red }}, {{ green }}, {{ blue }}, {{ alpha }})

    text: {{ textColor }}
    <input type="text" @input="transform" v-model="colorString">
    <div id="sliders">
      <span>Red</span>
      <color-slider @input="(e) => {sliderInput(e, 'red')}" v-model="red"></color-slider>
      <span>Green</span>
      <color-slider @input="(e) => {sliderInput(e, 'green')}" v-model="green"></color-slider>
      <span>Blue</span>
      <color-slider @input="(e) => {sliderInput(e, 'blue')}" v-model="blue"></color-slider>
      <span>Alpha</span>
      <color-slider @input="(e) => {sliderInput(e, 'alpha')}" v-model="alpha" is-alpha></color-slider>
    </div>
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
    changeColorString() {
      if (this.color.valpha === 1) return this.color.hex().toString();
      return this.color.hexa().toString();
    },
    textColor() {
      return this.color.luminosity() > 0.5 ? "black" : "white";
    }
  },
  methods: {
    transform(event) {
      let c = ColorString.get(event.target.value);
      if (c !== null) {
        this.color = Color(event.target.value);
        this.red = this.color.red();
        this.green = this.color.green();
        this.blue = this.color.blue();
        this.alpha = this.color.alpha();
      }
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

      this.colorString = this.changeColorString;
    },
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
  margin: 2rem;
  background-color: transparent;
  border: none;
  max-width: calc(100vw - 6rem);;
}

input[type=text]:focus {
  outline: none;
}
#sliders {
  display: grid;
  grid-template-columns: repeat(4, auto);
  grid-template-rows: repeat(2, auto);
  place-items: center;
  grid-auto-flow: column;
  gap: 1rem;
  margin-left: 2rem;
}
</style>
