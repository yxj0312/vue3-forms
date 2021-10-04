<template>
    <label :for="uuid" v-if="label">{{ label }}</label>
    <input
        v-bind="$attrs"
        :placeholder="label"
        class="field"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :id="uuid"
        :aria-describedby="error? `${uuid}-error` :  null"
    >
    <p
      v-if="error"
      class="errorMessage"
      :id="`${uuid}-error`"
    >
      {{ error }}
    </p>
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
    },
    error: {
      type: String,
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
