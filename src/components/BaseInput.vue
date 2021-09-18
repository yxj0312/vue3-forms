<template>
    <label>{{ label }}</label>
    <input
        :placeholder="label"
        class="field"
    >
</template>

<script>

export default {
    props: {
        label: {
            type: String,
            default: ''
        } 
    }
}
</script>
