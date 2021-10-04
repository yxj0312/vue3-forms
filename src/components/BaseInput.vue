<template>
    <label v-if="label">{{ label }}</label>
    <input
        v-bind="$attrs"
        :placeholder="label"
        class="field"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"

    >
</template>

<script>
import UniqueID from '../features/UniqueID'

export default {
  props: {
    label: {
      type: String,
      default: ''
    },
    modelValue: {
      type: [String, Number],
      default: ''
    }
  },
  setup () {
    const uuid = UniqueID().getID()

    return {
      uuid
    }
  }
}
</script>
