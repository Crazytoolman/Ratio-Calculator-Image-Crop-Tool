# Ratio Calculator & Image Crop & Stitch Tool

An elegant, modern online tool integrating **Aspect Ratio Calculation**, **Image Cropping**, and **Image Stitching**. Designed with a refined Apple-style aesthetic, supporting Dark Mode, English/Chinese localization, and optimized for mobile touch interactions.

[![English](https://img.shields.io/badge/Language-English-blue.svg)](./README_EN.md) [![Chinese](https://img.shields.io/badge/Language-中文-red.svg)](./README.md)

![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

## ✨ Features

### 🤖 AI Painting Companion (New!)
- **Draw Things Pre-processing**: Specially optimized for pre-processing images before AI generation. Through precise ratio cropping and rotation, ensuring input images perfectly match AI model dimensions, significantly improving output quality.
- **Draw Things Outpainting Base**: Specially designed for AI outpainting workflows. Quickly create a green background base that perfectly matches outpainting LoRAs (trained with pure green) in tools like Draw Things. Facilitates a "Layout -> Fill Green -> Paste & Gen" high-speed workflow.
- **Perfect for ACE++**: The new "Pre-stitch" workflow is ideal for models like **ACE (ACE++)**, allowing for pre-arranged multi-character or scene compositions on a single canvas as guided input.
- **Qwen Image Edit Assistant**: Tailored for multimodal editing models like **Qwen Image Edit**. Use the stitching feature to pre-layout relationships between multiple characters or scenes for precise in-painting and guided generation.

### 📐 Smart Ratio Calculator
- **Common Presets**: One-click selection for 16:9, 4:3, 1:1, 9:16 (Portrait), and more.
- **Custom Calculation**: Input width or height to automatically calculate the other, or input resolution to calculate the ratio.
- **Dynamic Preview**: Real-time visual preview box to intuitively feel the shape differences of various ratios.

### ✂️ Powerful Image Cropping
- **Versatile Uploads**: Supports click-to-upload, drag-and-drop, and **Ctrl+V paste**.
- **Smart Cropping**: LockAspectRatio cropping (supports 1:1, 16:9, etc.) or freeform adjustment.
- **Border Radius Control**:
  - **Edge Dragging**: Intuitively adjust radius using arrow indicators on all four edges.
  - **Precise Control**: Supports slider adjustment and precise numeric input.
  - **Quick Reset**: Double-click the slider to instantly reset radius to 0px.
- **Free Rotation**: 
  - Supports **90° step rotation**.
  - Supports **Slider free-angle rotation** (Double-click to reset to 0°).
  - **Gesture Support**: Two-finger rotation on mobile, two-finger rotation on Mac trackpad.
- **HD Export**: Download results in lossless PNG format.
- **Clipboard Integration**: One-click copy result (Perfectly compatible with iOS/Android and Desktop).

### 🧩 Image Stitching (New!)
- **Batch Upload**: Upload multiple images at once, or add images via paste/drag-and-drop.
- **Stitching Modes**: 
  - **Horizontal (Side-by-Side)**: Arrange images horizontally, auto-aligned by the tallest image height.
  - **Vertical (Top-to-Bottom)**: Stack images vertically, auto-aligned by the widest image width.
- **Drag-to-Reorder**: Drag images to rearrange order, with real-time stitching preview.
- **Pre-crop Support**: Crop/rotate individual images before adding them to the stitching queue.
- **Real-time Preview**: Displays live preview of stitched result and final resolution.
- **Flexible Output**: Download/copy stitched result directly, or continue editing as a new image.

### 🖼️ Outpainting Base (New!)
- **Smart Base Generation**: One-click generation of pure green (#00FF00) canvases in 4:3, 16:9, or custom ratios using the integrated calculator.
- **Multi-layer Interaction**: Add images via click, paste, or drag-and-drop to create custom layouts.
- **Precision Controls**: Freely move, scale, and rotate floating images for professional layer management.
- **Seamless Export**: Download or copy results directly, perfectly integrated with AI creators like Draw Things.


### 🎨 Ultimate UI/UX Experience
- **Apple-Style Design**: Glassmorphism, smooth animations, and refined shadow details.
- **Dark Mode**: Perfectly adapts to system appearance, with manual toggle support.
- **Internationalization (i18n)**: Real-time switching between **Simplified Chinese** and **English**.
- **Fully Responsive**: Perfectly adapted for Desktop, Tablet, and Mobile (Solved mobile UI overlapping issues).

## 🚀 Getting Started

### Method 1: Online Access (Recommended)
Access the officially deployed tool directly, ready to use on PC and Mobile:
👉 **[crop.toolbuddy.art](http://crop.toolbuddy.art)**

### Method 2: Local Execution
If you wish to use it in an offline environment:

1. Clone this repository:
   ```bash
   git clone https://github.com/Crazytoolman/Ratio-Calculator-Image-Crop-Tool.git
   ```
2. To ensure features like clipboard copying work correctly (due to browser security policies), it is recommended to run a local server:
   ```bash
   # Python
   python -m http.server 8000
   ```

## 📱 Mobile Optimization

Deeply customized for mobile user habits:
- **Touch Optimization**: Enlarged crop handles, two-finger rotation gestures.
- **UI Adaptation**: Smart layout adjustments to prevent top control bars from obscuring content.
- **Compatibility**: Solved clipboard write permission compatibility issues on iOS Safari.

## 🛠️ Tech Stack

- **Core**: HTML5, CSS3 (Flexbox/Grid), Vanilla JavaScript (ES6+).
- **Zero Dependencies**: No third-party frameworks (like React, Vue, jQuery), lightweight and fast.
- **Canvas API**: Used for high-performance image processing (Rotation, Scaling, Cropping).

## 👤 Author

**ToolBuddy**
- 📺 Bilibili: [Follow](https://space.bilibili.com/33410454)
- ▶️ YouTube: [Follow](https://www.youtube.com/@crazytoolman)
- 💬 WeChat: `crazytoolman`
- 🧡 WeChat Reward Code (Buy the author a tea):

<img src="./sponsor.jpg" alt="WeChat Reward Code" width="500" />

## 📄 License

This project is open-sourced under the **GPL-3.0 License**.
