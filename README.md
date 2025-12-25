# 比例计算器 & 图像裁切拼接工具 (Ratio Calculator & Image Crop & Stitch Tool)

一个优雅、现代化的在线工具，集成了**比例计算**、**图片裁切**与**图片拼接**功能。采用精致的 Apple 风格设计，支持深色模式、中英多语言切换，并针对移动端触控进行了深度优化。

[![English](https://img.shields.io/badge/Language-English-blue.svg)](./README_EN.md) [![Chinese](https://img.shields.io/badge/Language-中文-red.svg)](./README.md)

![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

## ✨ 功能特性 (Features)

### 🤖 AI 绘画伴侣 (New!)
- **Draw Things 预处理神器**: 特别优化用于 AI 绘画前的图片预处理。通过精确的比例裁切和旋转，确保输入图像完美契合 AI 模型的尺寸要求，显著提升出图质量。

### 📐 智能比例计算器
- **常用比例预设**: 一键选择 16:9, 4:3, 1:1, 9:16 (竖屏) 等常见比例。
- **自定义计算**: 输入宽或高，自动计算另一边长，或输入分辨率自动计算比例。
- **动态预览**: 实时可视化的比例预览框，直观感受不同比例的形状差异。

### ✂️ 强大的图像裁切
- **多方式上传**: 支持点击上传、拖拽上传以及 **Ctrl+V 粘贴**图片。
- **智能裁切**: 锁定比例裁切（支持 1:1, 16:9 等多种预设），或自由调整。
- **圆角调节**:
  - **边缘拖动**: 通过四条边的箭头指示器直观地调整圆角。
  - **精确控制**: 支持滑块调节与精确数值输入。
  - **快速重置**: 双击滑块可快速恢复 0px 圆角。
- **自由旋转**: 
  - 支持 **90° 步进旋转**。
  - 支持 **滑块由角度旋转**（支持双击滑块复位 0°）。
  - **手势支持**: 移动端双指旋转、Mac 触控板双指旋转。
- **高清导出**: 结果以 PNG 格式无损下载。
- **剪贴板集成**: 支持一键复制裁切结果（完美兼容 iOS/Android 及桌面端）。

### 🧩 图片拼接 (New!)
- **多图批量**: 支持同时上传多张图片，或分批粘贴/拖拽添加。
- **拼接模式**: 
  - **左右拼接（横向）**: 多图水平排列，自动按最高图片对齐高度。
  - **上下拼接（纵向）**: 多图垂直堆叠，自动按最宽图片对齐宽度。
- **拖拽排序**: 支持拖拽调整图片顺序，实时预览拼接效果。
- **预裁切支持**: 可先对单张图片进行裁切/旋转，再添加到拼接队列。
- **实时预览**: 实时显示拼接预览图及最终分辨率。
- **灵活操作**: 支持直接下载 / 复制拼接结果，或将拼接结果作为新图片继续编辑。


### 🎨 极致的 UI/UX 体验
- **Apple 风格设计**: 毛玻璃特效 (Glassmorphism)、流畅的动画过渡、精致的阴影细节。
- **深色模式 (Dark Mode)**: 完美适配系统外观，也可手动一键切换。
- **国际化 (i18n)**: 支持 **简体中文** 与 **English** 实时切换。
- **完全响应式**: 完美适配桌面端、平板及手机端（解决了移动端 UI 遮挡问题）。

## 🚀 快速开始 (Getting Started)

### 方法 1: 在线访问 (推荐)
直接访问官方部署的在线工具，电脑手机均可即开即用：
👉 **[crop.toolbuddy.art](http://crop.toolbuddy.art)**

### 方法 2: 本地运行
如果您希望在离线环境使用：

1. 下载或克隆本仓库：
   ```bash
   git clone https://github.com/Crazytoolman/Ratio-Calculator-Image-Crop-Tool.git
   ```
2. 为了确保剪贴板复制等功能正常工作，建议使用本地服务器运行：
   ```bash
   #Python
   python -m http.server 8000
   ```

## 📱 移动端优化

针对移动端用户习惯进行了深度定制：
- **触控优化**: 增大的裁切手柄，双指手势旋转。
- **UI 适配**: 智能调整布局，防止顶部控制栏遮挡内容。
- **兼容性**: 解决了 iOS Safari 下剪贴板写入权限的兼容性问题。

## 🛠️ 技术栈 (Tech Stack)

- **核心**: HTML5, CSS3 (Flexbox/Grid), Vanilla JavaScript (ES6+).
- **无依赖**: 不依赖任何第三方框架（如 React, Vue, jQuery），轻量极速。
- **Canvas API**: 用于高性能的图像处理（旋转、缩放、裁切）。

## 👤 作者 (Author)

**工具狂 (ToolBuddy)**
- 📺 Bilibili: [点击关注](https://space.bilibili.com/33410454)
- ▶️ YouTube: [点击关注](https://www.youtube.com/@crazytoolman)
- 💬 WeChat: `crazytoolman`
- 🧡 微信赞赏码（给作者买奶茶）:

<img src="./sponsor.jpg" alt="微信赞赏码" width="500" />

## 📄 许可证 (License)

本项目基于 **GPL-3.0 License** 开源。
