<template>
    <div v-if="error" class="alert alert-danger">{{ error }}</div>

    <div v-else-if="loading" class="text-center py-5 text-muted">
        <div class="spinner-border spinner-border-sm me-2" role="status"></div>
        {{ loadingText }}
    </div>

    <div v-else-if="isEmpty" class="card shadow-sm">
        <div class="card-body text-muted">{{ emptyText }}</div>
    </div>

    <slot v-else />
</template>

<script setup>
defineProps({
    error: {
        type: String,
        default: null,
    },
    loading: {
        type: Boolean,
        default: false,
    },
    isEmpty: {
        type: Boolean,
        default: false,
    },
    loadingText: {
        type: String,
        default: 'Loading...',
    },
    emptyText: {
        type: String,
        default: 'No records found.',
    },
});
</script>

