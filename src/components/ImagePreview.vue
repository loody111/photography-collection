<template>
    <div v-if="visible" class="image-preview-overlay" @click.self="closePreview">
        <div class="image-preview-content">
            <img :src="imageUrl" alt="Preview" class="preview-image" />
            <button class="close-button" @click="closePreview">×</button>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    imageUrl: String,
    visible: Boolean,
});

const emit = defineEmits(['close']);

const closePreview = () => {
    emit('close');
};
</script>

<style scoped>
.image-preview-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2000;
}

.image-preview-content {
    position: relative;
    max-width: 90%;
    max-height: 90%;
    background-color: var(--card-bg-color);
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    display: flex;
    justify-content: center;
    align-items: center;
}

.preview-image {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    border-radius: 4px;
}

.close-button {
    position: absolute;
    top: 10px;
    right: 10px;
    background: none;
    border: none;
    font-size: 24px;
    color: var(--text-color);
    cursor: pointer;
    padding: 5px;
    line-height: 1;
}

.close-button:hover {
    color: var(--nav-item-active-color);
}
</style>
