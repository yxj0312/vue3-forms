<template>
    <label>{{ label }}</label>
    <input 
        v-model="event.title"
        type="text" 
        placeholder="Title"
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
