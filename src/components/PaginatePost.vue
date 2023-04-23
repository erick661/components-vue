<script setup>
defineProps(["inicio", "fin", 'max'])
const emit = defineEmits(['next', 'prev'])


</script>
<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button type="button" class="btn btn-outline-primary" @click="emit('prev')" :disabled="inicio <= 0">Previus</button>
        <button type="button" class="btn btn-outline-primary" @click="emit('next')" :disabled="fin >= max">Next</button>
    </div>
</template>