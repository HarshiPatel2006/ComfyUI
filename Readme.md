<div align="center">
  <h1>🎨 ComfyUI - Categorized Pixaroma Nodes</h1>
  <p><i>A highly organized, structured version of Pixaroma nodes for an accelerated ComfyUI workflow.</i></p>
  
  ![ComfyUI](https://img.shields.io/badge/ComfyUI-Compatible-green?style=for-the-badge&logo=appveyor)
  ![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python&logoColor=white)
  ![License](https://img.shields.io/badge/License-MIT-purple?style=for-the-badge)
</div>

---

## 🚀 Overview

Welcome to my custom node repository! This project provides **categorised versions of pixaroma nodes which you can use** to dramatically speed up your everyday ComfyUI workflows. 

The original Pixaroma node pack is an incredibly powerful suite of tools, but navigating a massive list of unorganized nodes can break your creative flow. By reorganizing these utilities into logical, structured categories, this repository makes it much simpler to find the exact node you need in seconds.

---

## ✨ Key Features & Benefits

| Feature | Description |
| :--- | :--- |
| 🗂️ **Categorized Menus** | Nodes are split into intuitive folders (Image, Math, Video, 3D) rather than one giant list. |
| ⚡ **Faster Workflow** | Spend less time searching the right-click menu and more time building your generation pipelines. |
| ☁️ **Cloud & Local Ready** | Works flawlessly whether you are running locally on a Windows machine or deploying via cloud GPU instances. |
| 🧩 **Plug-and-Play** | Drops right into your `custom_nodes` folder with zero extra dependencies required. |

---

## 📦 Installation Guide

### Standard / Local Setup
If you are running ComfyUI locally, clone this repository directly into your custom nodes folder.

1. Open your command prompt or terminal.
2. Navigate to your ComfyUI custom nodes directory:
   ```bash
   cd ComfyUI/custom_nodes
   ```
3. Clone the repository:
   ```bash
   git clone https://github.com/HarshiPatel2006/ComfyUI.git
   ```
4. Restart your ComfyUI server. 

### Cloud Deployment (RunPod / Hosted Instances)
If you are deploying ComfyUI on a cloud GPU platform like RunPod, you can pull the repository directly into your workspace terminal before launching your ComfyUI instance:

```bash
cd /workspace/ComfyUI/custom_nodes
git clone https://github.com/HarshiPatel2006/ComfyUI.git
```
*Note: Ensure your cloud instance has Git installed and remember to restart the ComfyUI service after cloning so the nodes initialize properly.*

---

## 🔍 What's Inside?

<details>
<summary><b>🖼️ Image Utilities</b> (Click to expand)</summary>
Contains all nodes related to image cropping, resizing, background removal, layer masking, and compositing.
</details>

<details>
<summary><b>🧮 Math & Logic</b> (Click to expand)</summary>
Essential nodes for workflow automation, variable routing, condition checking, and dynamic value calculations.
</details>

<details>
<summary><b>🎵 Audio & Video</b> (Click to expand)</summary>
Tools designed for audio-reactivity generation, video frame extraction, and timeline sequencing.
</details>

<details>
<summary><b>🧊 3D Scene Building</b> (Click to expand)</summary>
Categorized tools for 3D rendering, object placement, and spatial generation within the ComfyUI environment.
</details>

---

## 💡 How to Use

Once installed, simply right-click anywhere on your ComfyUI canvas. Instead of a cluttered root menu, you will now see a dedicated, organized **Pixaroma Categorized** dropdown menu. Navigate through the sub-menus to drop your desired node directly into the workspace.

> **Tip:** You can still use the double-click search bar on the canvas to find these nodes instantly by typing their original names!

---

## 🙏 Credits & Acknowledgements

* **Original Node Development:** Massive thanks to the [Pixaroma Team](https://github.com/pixaroma/ComfyUI-Pixaroma) for creating the original underlying logic and core functions of these nodes. This repository is simply a structural reorganization of their excellent work.
* **Workflows & Tutorials:** Check out the official [Pixaroma Workflows site](https://workflows.pixaroma.com/) for examples of how to put these nodes to work in advanced pipelines.