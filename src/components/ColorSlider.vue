<script>
export default {
  props: {'modelValue': [String, Number], "isAlpha": Boolean},
  emits: ['update:modelValue']
}
</script>

<template>
  <input v-if="!isAlpha" type="range" min="0" max="255"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
  />

  <input v-else type="range" min="0" max="1" step="0.00390625"
         :value="modelValue"
         @input="$emit('update:modelValue', $event.target.value)"
  />
</template>