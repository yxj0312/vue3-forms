# Vue3 Forms

## L2 Base Input

## v-model: Emitting the update:modelValue event

All components that are capable of being v-modeled have to emit an event in order for the parent to be able to catch the updates to that component’s data.

In Vue 3, by default all v-model contracts expect for your component to emit an update:modelValue event, regardless of what type of input, or inputs, your component contains.

Let’s go ahead and add an input event listener to our <input/> element, and emit an update:modelValue event whenever an input event occurs.

BaseInput.vue

```javaScript
<template>
  <label v-if="label">{{ label }}</label>
  <input
    :value="modelValue"
    :placeholder="label"
    @input="$emit('update:modelValue', $event.target.value)"
    class="field"
  >
</template>

<script>
export default {
  props: {
    label: {
      type: 'String',
      default: ''
    },  
    modelValue: {
      type: [String, Number],
      default: ''
    }
  }
}
</script>
```
