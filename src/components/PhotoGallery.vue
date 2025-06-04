<template>
    <div class="photo-gallery">
        <h2>Photography Collection</h2>
        <div class="filters">
            <button @click="filterByGroup('all')" :class="{ active: currentGroup === 'all' }">所有图片</button>
            <button v-for="group in uniqueGroups" :key="group" @click="filterByGroup(group)" :class="{ active: currentGroup === group }">
                {{ group }}
            </button>
        </div>

        <div class="gallery-grid">
            <div v-for="photo in filteredPhotos" :key="photo.id" class="photo-item" @click="openPreview(photo.url)">
                <img :src="photo.url" :alt="photo.title" />
                <p class="photo-title">{{ photo.title }}</p>
                <span class="photo-group">{{ photo.group }}</span>
            </div>
        </div>

        <ImagePreview :image-url="previewImageUrl" :visible="isPreviewVisible" @close="closePreview" />

        <p v-if="filteredPhotos.length === 0">暂无图片可显示。</p>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import ImagePreview from './ImagePreview.vue';

// 动态导入图片
const imageModules = import.meta.glob('/src/assets/**/*.jpg', { eager: true, query: '?url', import: 'default' });

const photos = ref([]);

let idCounter = 1;
for (const path in imageModules) {
    const url = imageModules[path];
    const parts = path.split('/');
    const group = parts[parts.length - 2]; // 倒数第二个部分是分类名
    const title = parts[parts.length - 1].split('.')[0]; // 文件名作为标题

    photos.value.push({
        id: idCounter++,
        url: url,
        title: title,
        group: group,
    });
}

const currentGroup = ref('all');

const uniqueGroups = computed(() => {
    const groups = new Set(photos.value.map(photo => photo.group));
    return Array.from(groups);
});

const filteredPhotos = computed(() => {
    if (currentGroup.value === 'all') {
        return photos.value;
    } else {
        return photos.value.filter(photo => photo.group === currentGroup.value);
    }
});

const filterByGroup = group => {
    currentGroup.value = group;
};

const previewImageUrl = ref('');
const isPreviewVisible = ref(false);

const openPreview = url => {
    previewImageUrl.value = url;
    isPreviewVisible.value = true;
};

const closePreview = () => {
    isPreviewVisible.value = false;
    previewImageUrl.value = '';
};

// 假设图片上传后会调用此方法添加新图片
const addPhoto = newPhoto => {
    photos.value.push(newPhoto);
};

// 暴露给父组件的方法，如果需要的话
// defineExpose({ addPhoto });
</script>

<style scoped>
.photo-gallery {
    border-radius: 3px;
}

.photo-gallery h2 {
    display: none; /* 隐藏标题，因为App.vue中已经有主标题 */
}

.filters {
    display: flex;
    overflow-x: auto; /* 允许水平滚动 */
    white-space: nowrap; /* 防止内容换行 */
    -webkit-overflow-scrolling: touch; /* 改善iOS上的滚动体验 */
    padding: 10px 0; /* 调整padding */
    justify-content: flex-start; /* 左对齐，以便滚动 */
    border-bottom: 1px solid var(--border-color); /* 添加底部边框 */
}

.filters::-webkit-scrollbar {
    display: none; /* 隐藏滚动条 */
}

.filters button {
    background-color: transparent;
    border: none;
    padding: 10px 15px;
    margin: 0;
    cursor: pointer;
    font-weight: 600;
    border-bottom: 1px solid transparent; /* 使用bottom border作为激活指示 */
    border-radius: 2px; /* 使指示器边缘更圆润 */
    transition: all 0.3s ease;
    flex-shrink: 0; /* 防止按钮收缩 */
}

.filters button:hover {
    color: var(--nav-item-active-color);
}

.filters button.active {
    color: var(--nav-item-active-color);
    border-bottom-color: var(--nav-item-active-color);
    border-width: 2px; /* 激活时加粗指示器 */
}

.gallery-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* 3列布局 */
    gap: 2px; /* 减小间距 */
    margin-top: 20px;
}

.photo-item {
    overflow: hidden;
    position: relative;
    border-radius: 8px;
    padding-bottom: 100%; /* 保持正方形比例 */
}

.photo-item img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}

.photo-item .photo-title,
.photo-item .photo-group {
    display: none; /* 暂时隐藏标题和分组信息，类似Instagram的简洁展示 */
}

/* 响应式设计 */
@media (max-width: 768px) {
    .gallery-grid {
        grid-template-columns: repeat(3, 1fr); /* 移动端也保持3列 */
    }
}

@media (max-width: 480px) {
    .gallery-grid {
        grid-template-columns: repeat(2, 1fr); /* 更小的屏幕显示2列 */
    }
}
</style>
