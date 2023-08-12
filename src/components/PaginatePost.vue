<script setup>
    defineProps(["inicio", "fin", "total"]);
    const emit = defineEmits(["next", "prev"]);
</script>

<template>
    <div class="btn-group mb-2" role="group" aria-label="Basic example">
        <button
            @click="emit('prev')"
            type="button" 
            class="btn btn-outline-dark"
            :disabled="inicio <= 0"
        >
            Prev
        </button>

        <button
            @click="emit('next')" 
            type="button" 
            class="btn btn-outline-dark"
            :disabled="fin >= total"
        >
            Next
        </button>
    </div>
</template>