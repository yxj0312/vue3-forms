<template>
    <label>{{ label }}</label>
    <input
        :placeholder="label"
        class="field"
        :value="modelValue"

    >
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
        }
    }
}
</script>
