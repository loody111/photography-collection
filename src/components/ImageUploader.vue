<template>
    <div class="image-uploader">
        <h2>上传图片</h2>
        <input type="file" @change="handleFileChange" multiple accept="image/*" />
        <button @click="uploadImages" :disabled="!selectedFiles.length">开始上传</button>
        <p v-if="uploadMessage">{{ uploadMessage }}</p>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const selectedFiles = ref([]);
const uploadMessage = ref('');

const handleFileChange = event => {
    selectedFiles.value = Array.from(event.target.files);
    uploadMessage.value = '';
};

const uploadImages = () => {
    if (selectedFiles.value.length === 0) {
        uploadMessage.value = '请选择要上传的图片。';
        return;
    }

    // 模拟上传过程
    uploadMessage.value = `正在上传 ${selectedFiles.value.length} 张图片...`;
    setTimeout(() => {
        // 在实际应用中，这里会发送图片到后端服务器
        // 假设上传成功，并清空文件选择
        console.log(
            '上传的图片:',
            selectedFiles.value.map(file => file.name)
        );
        uploadMessage.value = `成功上传 ${selectedFiles.value.length} 张图片！`;
        selectedFiles.value = [];
        // 可以触发一个事件，通知父组件图片已上传，以便更新图片列表
        // emit('imagesUploaded', uploadedImageInfo);
    }, 2000);
};
</script>

<style scoped>
.image-uploader {
    margin-top: 20px;
    padding: 20px;
    border-radius: 3px;
    text-align: center;
}

.image-uploader h2 {
    margin-bottom: 15px;
}

.image-uploader input[type='file'] {
    margin-bottom: 15px;
    padding: 8px;
    border-radius: 3px;
    width: calc(100% - 16px); /* Adjust for padding */
    box-sizing: border-box;
}

.image-uploader button {
    padding: 10px 20px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    transition: background-color 0.3s ease;
}

.image-uploader p {
    margin-top: 15px;
    font-size: 14px;
}

/* 响应式调整 */
@media (max-width: 768px) {
    .image-uploader {
        padding: 15px;
    }

    .image-uploader input[type='file'] {
        font-size: 14px;
    }

    .image-uploader button {
        padding: 8px 15px;
        font-size: 12px;
    }
}
</style>
