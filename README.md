# 我的摄影作品集

这是一个基于 Vue 3 和 Vite 构建的个人摄影作品集网站。它能够动态地从 `src/assets` 目录中读取图片，并根据子目录自动分类。

## 功能

- **动态图片加载**：自动扫描 `src/assets` 目录及其子目录下的 `.jpg` 图片。
- **自动分类**：根据图片所在的子目录自动将其归类（例如，`pet`、`portrait`、`scenery`）。
- **图片预览**：点击图片可查看大图预览。
- **响应式布局**：在不同设备上提供良好的浏览体验。
- **分类筛选**：通过顶部的分类按钮快速筛选图片。

## 项目结构

```
my-photo-gallery/
├── public/
│   └── photo.svg
├── src/
│   ├── App.vue
│   ├── assets/             # 存放图片资源，按子目录分类
│   │   ├── pet/
│   │   ├── portrait/
│   │   └── scenery/
│   ├── components/         # Vue 组件
│   │   ├── HeaderNav.vue
│   │   ├── ImagePreview.vue
│   │   ├── ImageUploader.vue
│   │   └── PhotoGallery.vue # 核心图片展示组件
│   ├── main.js             # 应用入口文件
│   └── style.css           # 全局样式
├── index.html              # 主 HTML 文件
├── package.json            # 项目依赖和脚本
└── vite.config.js          # Vite 配置
```

## 安装与运行

请确保您已安装 [Node.js](https://nodejs.org/) 和 [npm](https://www.npmjs.com/)。

1.  **克隆仓库**：

    ```bash
    git clone <您的仓库地址>
    cd my-photo-gallery
    ```

2.  **安装依赖**：

    ```bash
    npm install
    ```

3.  **开发模式运行**：

    ```bash
    npm run dev
    ```

    项目将在 `http://localhost:5173/` 启动。

4.  **构建生产版本**：

    ```bash
    npm run build
    ```

    构建后的文件将输出到 `dist` 目录。

## 如何添加新图片

只需将您的 `.jpg` 图片文件放置到 `src/assets` 目录下的相应子目录中即可。例如，如果您想添加一张宠物图片，可以将其放入 `src/assets/pet/` 目录。项目会自动检测并显示新添加的图片。

## 技术栈

-   [Vue 3](https://vuejs.org/) - 渐进式 JavaScript 框架
-   [Vite](https://vitejs.dev/) - 快速的下一代前端工具

## 许可证

[MIT License](LICENSE)
