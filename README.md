<div align="center">
  <img width="1200" alt="InfiniteMotion Studio Banner" src="" style="border-radius: 12px; margin-bottom: 20px;" />

  # 🎬 InfiniteMotion Studio 2.1

  **A sophisticated spatial video editor with an infinite canvas, virtual camera, mesh gradients, and smart intersection triggers.**

  [![React](https://img.shields.io/badge/React-19.2-blue.svg?style=for-the-badge&logo=react)](https://reactjs.org/)
  [![TypeScript](https://img.shields.io/badge/TypeScript-5.8-blue.svg?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
  [![Vite](https://img.shields.io/badge/Vite-6.2-646CFF.svg?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
  [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)


---

## ✨ Key Features

InfiniteMotion Studio reimagines video editing by moving away from static frames and into a dynamic, spatial canvas. 

*   🎥 **Infinite Spatial Canvas & Virtual Camera:** Animate a virtual camera (X, Y, Zoom, Rotation) across an infinite 2D workspace to create dynamic, sweeping cinematic sequences.
*   ⚡ **Smart Intersection Triggers:** Assets automatically trigger entrance and exit animations (e.g., zoom-reveal, elastic-scale, blur-fade) precisely when the virtual camera pans over them.
*   🎛️ **Professional Timeline:** A robust multi-track timeline featuring magnetic snapping, marquee selection, precision playhead scrubbing, and seamless keyframe drag-and-drop synchronization.
*   🚀 **Hardware-Accelerated Export:** Utilizes native WebCodecs (`VideoEncoder`, `AudioEncoder`) alongside `mp4-muxer` and `webm-muxer` for blazing-fast, frame-perfect MP4 (H.264) and WebM (VP9) rendering.
*   🎨 **Advanced Visual Styling:** Built-in support for animated mesh gradients, frosted glassmorphism, CRT scanlines, film grain, and advanced typography (including full Arabic font support).
*   🎵 **Rich Media Support:** Mix and manipulate text, images, HTML code blocks, video, and audio assets with independent volume controls and spatial masking.

---

## 🛠️ Tech Stack

| Category | Technologies |
| :--- | :--- |
| **Frontend Framework** | React 19, ReactDOM |
| **Language & Build** | TypeScript, Vite |
| **Styling & UI** | Tailwind CSS, Lucide React (Icons) |
| **Rendering & Export** | HTML5 Canvas API, WebCodecs API, `mp4-muxer`, `webm-muxer`, `html2canvas` |


---

## 💡 Usage Guide

1. **Set the Stage:** Use the **Inspector** (right panel) to set your project resolution, framerate, and background style (try the animated *Mesh Gradient*!).
2. **Add Assets:** Use the left sidebar to drop Text, Images, Videos, Audio, or AI-generated HTML Code onto the canvas.
3. **Animate the Camera:** Move the playhead on the **Timeline**, adjust your viewport (pan/zoom/rotate), and click the **+ Camera KF** button to lay down a camera path.
4. **Smart Triggers:** Select an asset and assign an `In` and `Out` animation in the Inspector. The asset will automatically animate when the camera "sees" it.
5. **Preview & Export:** Hit the **Preview** button for a real-time cinematic playback, then click **Export** to bake your creation into a high-fidelity MP4 or WebM file.

---


## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---
*Built for creators and motion designers.*
