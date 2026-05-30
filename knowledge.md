# 项目技术知识库 (Ratio Calculator & Image Crop Tool Technical Knowledge Base)

本知识库（`knowledge.md`）旨在记录本项目的整体架构、设计方案、核心模块实现细节（包括比例计算器的隔离方案、扩图底板的机制、移动端与手势的适配以及剪贴板兼容性），以帮助后续所有 AI 助手与开发上下文能够快速理解本项目的部署并做出正确的维护与更新。

---

## 📂 1. 项目整体架构与设计方案

### 1.1 技术栈 (Vanilla Tech Stack)
本项目是一个纯前端、无依赖的单页应用（SPA）：
*   **核心**: HTML5、CSS3 (Flexbox/Grid 布局) 与原生 JavaScript (ES6+)。
*   **设计原则**: 严禁引入任何第三方框架（如 React、Vue、jQuery）或大型 UI 库，保证极致的页面加载速度和轻量化。
*   **图像引擎**: 原生 Canvas API，用于高性能的图像旋转、缩放、圆角应用、拼接与合成。

### 1.2 设计方案与视觉系统 (Apple Aesthetics)
*   **毛玻璃特效 (Glassmorphism)**: 广泛应用 CSS `backdrop-filter: blur()` 与半透明背景色、精致的内发光边框与动态阴影，营造高端的 Apple 视觉风格。
*   **深色/浅色模式 (Theme Toggle)**: 
    *   通过 CSS 全局变量（Variables）管理色彩系统。
    *   点击切换按钮或读取系统配置时，向 `document.documentElement` 添加/移除 `data-theme="dark"`，实现无缝主题过渡。
*   **国际化多语言系统 (i18n)**:
    *   在 HTML 元素上使用 `data-i18n` (翻译文本) 和 `data-i18n-placeholder` (输入框提示) 标签。
    *   通过内置的 `translations` 字典对象进行全词条替换，支持**简体中文**与**English**实时无刷新切换。

---

## 📐 2. 比例计算器模块的分配与隔离方案

### 2.1 比例计算器在各模块中的分配
比例计算器是本工具的核心基石，它不仅是一个独立的工具，还作为尺寸预设的计算层服务于以下三大模块：
1.  **单图裁切与拼接**：计算裁切框的锁定高宽比，并更新裁切框尺寸。
2.  **批量裁切**：定义批量图像裁切的网格锁定比例。
3.  **扩图底板**：计算出绿幕底板的实际导出像素尺寸（如 `1200 x 900` 等符合比例的尺寸）。

### 2.2 比例计算器的隔离方案 (State Isolation Scheme)
为了确保用户在不同功能标签页（Tab）切换时，各自的比例状态不发生混淆和覆盖，项目采取了**状态隔离与局部同步结合**的方案：

*   **数据隔离**:
    *   **裁切与拼接模式** 使用全局变量 `currentRatio = { width: X, height: Y }` 以及对应的 DOM 比例选择卡片 `#ratioSelectCard`。
    *   **扩图底板模式** 使用专门的独立变量 `opCurrentRatio = { width: X, height: Y }` 以及独立的 DOM 比例选择卡片 `#opRatioSelectCard`。
*   **事件与交互隔离**: 两套比例卡片各自拥有独立的按钮组监听器，其激活状态（`active` class）和自定义宽高的交换机制（Swap）互不干扰。
*   **平滑切换同步 (Smooth Synchronization)**:
    *   为了提升用户体验，在标签切换机制中（`tool === 'outpainting'`），会自动执行一次浅拷贝同步：`opCurrentRatio = { ...currentRatio }`，并将裁切端的比例状态带入到底板端，同时刷新底板的比例 UI 按钮激活态。
    *   在此之后，用户在底板端做出的任何比例修改只作用于 `opCurrentRatio`，不再反向影响裁切端的 `currentRatio`，从而实现了“**智能带入，相互独立**”的隔离效果。

---

## 🖼️ 3. 扩图底板的作用与实现细节

### 3.1 扩图底板的作用
扩图底板专门为 AI 绘画（如 **Draw Things** 的 Outpainting 扩图 LoRA，或 **Qwen Image Edit** 等多模态重绘模型）设计：
1.  **色键扣像准备**: 自动生成符合特定比例、且颜色严格为 **纯绿（Chroma Green, `#00FF00`）** 的画布底板，不夹杂任何偏色或渐变，以便 AI 软件进行高精度的色键绿幕识别。
2.  **自由布局空间**: 允许用户在其上粘贴、拖拽、旋转并缩放一张或多张图片，快速排出主图与扩图区域的相对空间关系。

### 3.2 实现细节 (Implementation Details)
扩图底板的渲染与合成包含“**交互层**”与“**Canvas 物理绘制层**”两层结构：

```
+---------------------------------------------------------+
| [交互层] #outpaintingOverlay                             |
|  - 管理多个 HTML 悬浮图层元素 (.outpainting-item)       |
|  - 捕获 Mouse/Touch 事件，实时应用 CSS transform        |
+---------------------------------------------------------+
                            │ (坐标转换与缩放比 scaleRatio)
                            ▼
+---------------------------------------------------------+
| [Canvas 物理层] #outpaintingCanvas                     |
|  - 1. 填充纯绿色底色 ctx.fillRect(0, 0, w, h)          |
|  - 2. ctx.save() -> ctx.translate() -> ctx.rotate()     |
|  - 3. ctx.drawImage() 按物理像素比例无损写入           |
+---------------------------------------------------------+
```

*   **HTML 交互悬浮层**:
    *   底板容器内包含 `#outpaintingOverlay` 交互层，用户粘贴或拖入的每一张图片都会生成一个对应的子元素 `.outpainting-item`。
    *   **默认居中满屏贴合 (Contain Fit)**：图片加载时，自动计算与底板宽高对应的 `scale = Math.min(scaleX, scaleY)`。初始化时图片将完美贴合底板边缘（顶高或贴宽）并居中，不再粗暴地等比缩小至 80%。
    *   **填充宽度 (Fill Width)**：重置旋转并强制图片宽度拉伸至底板宽度，高按比例调整，限制仅可上下拖拽（`left` 锁定为 `0px`）。
    *   **填充高度 (Fill Height)**：重置旋转并强制图片高度拉伸至底板高度，宽按比例调整，限制仅可左右拖拽（`top` 锁定为 `0px`）。
    *   **拖动轴向锁定 (lockDir)**：在元素上通过 `dataset.lockDir` 记录锁定方向（`'x'` 或 `'y'`）。拖动时在 `handleOutpaintingMove` 中拦截非锁定坐标轴。任何手动缩放、旋转或手势修改均会隐式调用 `delete el.dataset.lockDir` 从而即时清除该方向锁定。
    *   通过监听拖拽、角点缩放以及角度控制条，修改子元素的 `style.left`、`style.top`、`style.width`、`style.height` 以及自定义属性 `data-rotation`。
*   **Canvas 像素合成渲染 (`renderMergedOutpainting`)**:
    *   在导出或确认合成时，程序根据 Canvas 的真实物理像素宽度（如 `2048px`）与当前底板在页面上的 CSS 视觉呈现宽度，计算出缩放比例因子：`scaleRatio = canvasWidth / visualWidth`。
    *   遍历所有 `.outpainting-item` 子元素，读取它们在交互层中的相对坐标与旋转角度。
    *   在 Canvas 上使用 `save()` 暂存状态，通过 `translate(centerX * scaleRatio, centerY * scaleRatio)` 将绘图原点移动到图片中心，调用 `rotate(rotation)` 进行旋转，最后调用 `drawImage()` 完成无损合成，并 `restore()` 恢复画布状态。

---

## 📱 4. 移动端适配与手势操作方案

为了提供可比拟原生 App 的移动端操作体验，项目对触控交互进行了深度适配：

### 4.1 双指旋转与缩放手势 (Multi-touch Gestures)
除了通过滑动条调节外，项目还在 Canvas 交互层实现了流畅的双指手势控制：
*   **触屏 Touch 事件绑定**: 监听 `touchstart`、`touchmove` 和 `touchend` 事件，并将 `{ passive: false }` 显式传给事件监听器，以便能成功调用 `e.preventDefault()` 阻止移动端浏览器的默认回弹与滚动行为。
*   **手势数值计算**:
    1.  当检测到 `e.touches.length === 2` 时，计算两点间的欧氏距离：
        $$\text{distance} = \sqrt{\Delta x^2 + \Delta y^2}$$
    2.  计算双指连线的正切夹角角度：
        $$\text{angle} = \arctan2(\Delta y, \Delta x) \times \frac{180}{\pi}$$
    3.  在手势移动过程中，通过与起始状态（`gestureStartDist` 和 `gestureStartAngle`）的差值计算出缩放比例 `scale` 和旋转角度增量 `angleDelta`，并应用至被操作图层的 CSS transform 和物理数据中。
*   **macOS 触控板适配 (Gesture API)**: 专门绑定了 Webkit 独有的 `gesturestart`、`gesturechange` 和 `gestureend` 事件，使得 Mac 触控板的双指捏合与旋转可以直接流畅地对裁切框或底板元素进行操作。

### 4.2 移动端 UI 避让机制
*   通过 CSS Media Queries 针对小屏幕设备进行深度定制，折叠非核心配置。
*   在检测到移动端触控行为时，动态计算并缩小裁切预览区的最大高度限制，防止软键盘或移动端底栏遮挡核心的裁切控制手柄。

---

## 📋 5. 剪贴板 iOS Safari 兼容工作流

在网页端实现一键复制 Canvas 导出的 PNG 图片到剪贴板，是一个极易踩坑的场景，特别是在 iOS Safari 浏览器中存在极严苛的安全限制。

### 5.1 iOS Safari 剪贴板写入限制原理
iOS Safari 要求 `navigator.clipboard.write()` 必须在**用户交互事件（如 `click`）的同步调用栈内**触发。如果在点击事件后执行了异步等待（如 `await imageBlob` 或者是等待 Canvas 的异步渲染），Safari 会判定该行为不再是由“用户手势直接触发”，从而直接静默拒绝或抛出 `NotAllowedError`。

### 5.2 兼容性解决方案 (User Gesture Workaround)
本项目采用了**同步声明 Promise + 延迟装载**的机制完美解决此问题：

```javascript
// 1. 在 click 事件的同步栈内，立即创建一个未决议的 Promise 包装器
const canvasPromise = createStitchCanvas(); // 返回 Promise<HTMLCanvasElement>

// 2. 将 Promise 转换成 Blob Promise
const blobPromise = canvasPromise.then(canvas => {
    return new Promise(resolve => canvas.toBlob(resolve, 'image/png'));
});

// 3. 极其关键：同步构建 ClipboardItem 并传入 blobPromise 占位，立即调用 write API！
// 此时 navigator.clipboard.write([item]) 是在 click 监听器内部同步执行的，Safari 予以放行。
const item = new ClipboardItem({ 'image/png': blobPromise });
navigator.clipboard.write([item]).then(() => {
    // 复制成功反馈
}).catch(err => {
    // 兜底降级方案：若极其陈旧的设备依然失败，则走自动触发下载逻辑
});
```

*   通过直接把 `Promise<Blob>` 传给 `ClipboardItem`，而不是 `await` 等到 Blob 产生后再调用 `ClipboardItem`，实现了**在不阻塞主线程的同时，顺应 Safari 的手势安全检查**。
*   如果某些特别老旧的设备不支持该异步剪贴板 API，系统将在 `catch` 分支中优雅地降级为自动触发 PNG 文件下载，确保用户流程不中断。
