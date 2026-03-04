<template>
    <div class="d-flex justify-content-between align-items-center mb-4">
        <h2 class="mb-0">{{ title }}</h2>
        <AppButton variant="primary" @click="$emit('add')">
            <slot name="icon" /> {{ addLabel }}
        </AppButton>
    </div>
</template>

<script setup>
import AppButton from './AppButton.vue';

defineProps({
    title: {
        type: String,
        required: true,
    },
    addLabel: {
        type: String,
        required: true,
    },
});

defineEmits(['add']);
</script>

