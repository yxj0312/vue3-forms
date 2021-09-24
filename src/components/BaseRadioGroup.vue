<template>
    <BaseRadio
        v-for="option in options"
    />
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    }
  }
}
</script>
