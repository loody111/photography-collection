<script setup>
import { ref, onMounted, watch } from 'vue';
import ImageUploader from './components/ImageUploader.vue';
import PhotoGallery from './components/PhotoGallery.vue';
import HeaderNav from './components/HeaderNav.vue';

const theme = ref(localStorage.getItem('theme') || 'light');

const toggleTheme = () => {
    theme.value = theme.value === 'light' ? 'dark' : 'light';
};

watch(theme, newTheme => {
    document.body.className = newTheme + '-theme';
    localStorage.setItem('theme', newTheme);
});

onMounted(() => {
    document.body.className = theme.value + '-theme';
});
</script>

<template>
    <div id="app">
        <HeaderNav @toggle-theme="toggleTheme" :current-theme="theme" />
        <div class="main-content">
            <!-- <ImageUploader /> -->
            <PhotoGallery />
        </div>
    </div>
</template>

<style>
/* App.vue 内部样式，主要用于布局 */
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.main-content {
    flex-grow: 1;
    padding: 20px;
    max-width: 935px;
    margin: 0 auto;
    width: 100%;
    box-sizing: border-box;
}

/* 响应式设计 */
@media (max-width: 768px) {
    .main-content {
        padding: 10px;
    }
}
</style>
