<script>
export default {
  props: {'modelValue': [String, Number], mode: String},
  emits: ['update:modelValue'],
  computed: {
    max() {
      switch (this.mode) {
        case "alpha":
          return 1;
        case "hue":
          return 360;
        case "satlight":
          return 100;
        default:
          return 255;
      }
    },
    step() {
      switch (this.mode) {
        case "alpha":
          return 0.00390625;
        default:
          return 1;
      }
    },
  },
}
</script>

<template>
  <input type="range" min="0" :max="max" :step="step"
         :value="modelValue"
         @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<style scoped>
input {
  --width: 30vmin;
  transform: rotate(270deg);
  width: var(--width);
  height: 1vmin;
  margin-inline: calc(-1 * var(--width));
  margin-block: calc(var(--width) / 2);
}
</style>