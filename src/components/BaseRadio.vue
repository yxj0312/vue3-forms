<template>
    <input
        type="radio"
        v-model="event.pets"
        :value="1"
        name="pets"
        />
    <label v-if="label">{{ label }}</label>
</template>

<script>

export default {
  props: {
    label: {
      type: String,
      default: ''
    },
    modelValue: {
      type: [String, Number],
      default: ''
    },
    value: {
      type: [String, Number],
      required: true
    }
  }
}
</script>
