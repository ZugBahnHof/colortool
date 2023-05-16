<template>
  <div id="color-window" :style="{backgroundColor: color, color: textColor}">

    back: rgba({{ red }}, {{ green }}, {{ blue }}, {{ alpha }})

    text: {{ textColor }}
    <input type="text" @input="transform" v-model="colorString">
    <div id="mode-switcher">
      <input type="radio" name="mode" value="hex" id="hex" v-model="mode">
      <label for="hex">HEX</label>
      <input type="radio" name="mode" value="hsl" id="hsl" v-model="mode">
      <label for="hsl">HSL</label>
    </div>
    <div class="slider-box">
      <input type="hidden" name="mode" :value="mode">
      <div class="sliders sliders-hex">
        <span>Red</span>
        <color-slider @input="(e) => {sliderInput(e, 'red')}" v-model="red"></color-slider>
        <span>Green</span>
        <color-slider @input="(e) => {sliderInput(e, 'green')}" v-model="green"></color-slider>
        <span>Blue</span>
        <color-slider @input="(e) => {sliderInput(e, 'blue')}" v-model="blue"></color-slider>
        <span>Alpha</span>
        <color-slider @input="(e) => {sliderInput(e, 'alpha')}" v-model="alpha" mode="alpha"></color-slider>
      </div>
      <div class="sliders sliders-hsl">
        <span>Hue</span>
        <color-slider @input="(e) => {sliderInput(e, 'hue')}" v-model="hue" mode="hue"></color-slider>
        <span>Sat.</span>
        <color-slider @input="(e) => {sliderInput(e, 'saturation')}" v-model="saturation"
                      mode="satlight"></color-slider>
        <span>Light</span>
        <color-slider @input="(e) => {sliderInput(e, 'lightness')}" v-model="lightness" mode="satlight"></color-slider>
        <span>Alpha</span>
        <color-slider @input="(e) => {sliderInput(e, 'alpha')}" v-model="alpha" mode="alpha"></color-slider>
      </div>
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
  mounted() {
    window.addEventListener("hashchange", this.handleHashChange)
  },
  unmounted() {
    window.removeEventListener("hashchange", this.handleHashChange)
  },
  data() {
    let hash = window.location.hash.replace("%23", "");
    let hashColor = ColorString.get(hash);
    let tempColor;

    if (hash && hashColor !== null) {
      tempColor = Color(hash);
    } else {
      tempColor = Color("#da1f3d");
    }
    window.location.hash = tempColor.hexa().toString();

    return {
      color: tempColor,
      colorString: tempColor.hexa().toString(),
      red: tempColor.red(),
      green: tempColor.green(),
      blue: tempColor.green(),
      hue: tempColor.hue(),
      saturation: tempColor.saturationl(),
      lightness: tempColor.lightness(),
      alpha: tempColor.alpha(),
      mode: "hex",
      hashTimer: null,
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
    setHash() {
      if (!this.hashTimer) {
        this.hashTimer = setTimeout(() => {
          this.hashTimer = null;
          window.location.hash = this.changeColorString;
        }, 500);
      }
    },
    transform(event) {
      let c = ColorString.get(event.target.value);
      if (c !== null) {
        this.color = Color(event.target.value);
        this.red = this.color.red();
        this.green = this.color.green();
        this.blue = this.color.blue();
        this.alpha = this.color.alpha();
        this.hue = this.color.hue();
        this.saturation = this.color.saturationl();
        this.lightness = this.color.lightness();
        this.setHash();
      }
    },
    handleHashChange() {
      console.log("Hash changed");
      let hash = window.location.hash.replace("%23", "") || this.colorString || "#da1f3d";
      let c = ColorString.get(hash);
      if (c !== null) {
        this.color = Color(hash);
        this.red = this.color.red();
        this.green = this.color.green();
        this.blue = this.color.blue();
        this.alpha = this.color.alpha();
        this.hue = this.color.hue();
        this.saturation = this.color.saturationl();
        this.lightness = this.color.lightness();
      }
      this.setHash();
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

        case "hue":
          if (0 <= value <= 360) this.color = this.color.hue(value);
          break;

        case "saturation":
          if (0 <= value <= 100) this.color = this.color.saturationl(value);
          break;

        case "lightness":
          if (0 <= value <= 100) this.color = this.color.lightness(value);
          break;
      }

      this.setHash();
      this.colorString = this.changeColorString;
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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

.sliders {
  display: grid;
  grid-template-columns: repeat(4, auto);
  grid-template-rows: repeat(2, auto);
  place-items: center;
  grid-auto-flow: column;
  gap: 1rem;
  transition: all 0.5s ease-in-out;
}

.slider-box {
  overflow: hidden;
  display: flex;
  margin-left: 2rem;
}

#mode-switcher {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-left: 2rem;
  gap: 1em;
}

input[name=mode][value=hex] {
  & ~ .sliders-hsl {
    transform: translate(100%, 0);
  }

  & ~ .sliders-hex {
    transform: translate(0, 0);
  }
}

input[name=mode][value=hsl] {
  & ~ .sliders {
    transform: translate(-200%, 0);
  }

  & ~ .sliders-hsl {
    transform: translate(-100%, 0);
  }
}

</style>
