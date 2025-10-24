# MediaPipe Optimized Build Matrix

Pre-compiled, optimized MediaPipe binaries for all major platforms with CPU and GPU delegate support.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: Windows](https://img.shields.io/badge/Platform-Windows-blue)](https://www.microsoft.com/windows)
[![Platform: Linux](https://img.shields.io/badge/Platform-Linux-orange)](https://www.linux.org/)
[![Platform: macOS](https://img.shields.io/badge/Platform-macOS-lightgrey)](https://www.apple.com/macos/)

## 🎯 Overview

This repository contains optimized MediaPipe binaries for **Windows, Linux, and macOS**, providing:
- **CPU-optimized builds** for specific architectures
- **GPU-accelerated builds** with OpenGL ES/Metal delegates
- **Complete C++ API access** - use shared libraries directly in your applications
- **Cross-platform support** - consistent API across all platforms
- **Task-specific modules** - Vision, GenAI/LLM, Text, Audio

Built from: [google/mediapipe](https://github.com/google/mediapipe) with build optimizations from [ocentra/mediapipe](https://github.com/ocentra/mediapipe)

---

## 📦 MediaPipe Components

### Task APIs Available

| API Category | Tasks | C++ Support | Delegates |
|--------------|-------|-------------|-----------|
| **Vision** | Object Detection, Image Classification, Face Detection, Pose Estimation, Hand Tracking, Image Segmentation | ✅ | CPU, GPU (OpenGL ES/Metal) |
| **GenAI / LLM** | Text Generation (Gemini, LLaMA, etc.) | ✅ | CPU only (desktop) |
| **Text** | Text Classification, Language Detection | ✅ | CPU, GPU |
| **Audio** | Audio Classification | ✅ | CPU, GPU |

> **Note:** Desktop LLM inference uses **CPU-only** delegates. GPU support for LLM is available on mobile platforms.

---

## 🛠️ GPU Delegate Support

| Platform | CPU | GPU Delegate | Notes |
|----------|-----|--------------|-------|
| **Windows** | ✅ | OpenGL ES | Via ANGLE, uses DirectX backend |
| **Linux** | ✅ | OpenGL ES | Native OpenGL support |
| **macOS** | ✅ | Metal | Apple Silicon (M1/M2/M3/M4) + Intel Macs |
| **Mobile** | ✅ | OpenGL ES (Android), Metal (iOS) | Mobile-only features |

> **Important:** MediaPipe Tasks API does **NOT** use CUDA/Vulkan on desktop. GPU acceleration is via OpenGL ES (Windows/Linux) or Metal (macOS).

---

## 📂 Directory Structure

```
MediaPipeRelease/
├── cpu/
│   ├── windows/                      # Windows CPU builds
│   │   ├── standard/                 # Standard CPU build
│   │   └── optimized-*/              # Architecture-specific builds (TBD)
│   ├── linux/                        # Linux CPU builds
│   │   ├── standard/                 # Standard CPU build
│   │   └── optimized-*/              # Architecture-specific builds (TBD)
│   └── macos/                        # macOS CPU builds
│       ├── arm/                      # Apple Silicon (M1/M2/M3/M4)
│       └── intel/                    # Intel Macs
│
└── gpu/
    ├── windows/                      # Windows GPU builds
    │   └── opengl-es/                # OpenGL ES via ANGLE
    ├── linux/                        # Linux GPU builds
    │   └── opengl-es/                # OpenGL ES native
    └── macos/                        # macOS GPU builds
        └── metal/                    # Metal GPU acceleration
```

---

## 🚀 Quick Start

### 1. Choose Your Platform & Build

**Windows:**
```powershell
cd cpu\windows\standard
# Or for GPU:
cd gpu\windows\opengl-es
```

**Linux:**
```bash
cd cpu/linux/standard
# Or for GPU:
cd gpu/linux/opengl-es
```

**macOS (Apple Silicon):**
```bash
cd cpu/macos/arm
# Or for GPU:
cd gpu/macos/metal
```

### 2. Use in Your Application

**C++ Example (Vision Task):**
```cpp
#include "mediapipe/tasks/cc/vision/object_detector/object_detector.h"

using namespace mediapipe::tasks::vision;

// Create object detector
auto options = ObjectDetectorOptions();
options.base_options.model_asset_path = "model.task";
options.base_options.delegate = BaseOptions::Delegate::GPU; // or CPU

auto detector = ObjectDetector::Create(std::move(options));

// Detect objects
auto results = detector->Detect(image);
```

**C++ Example (LLM Inference):**
```cpp
#include "mediapipe/tasks/cc/genai/inference/c/llm_inference_engine.h"

// Create LLM engine (CPU only on desktop)
LlmInferenceEngine_Session session;
LlmInferenceEngine_CreateEngine(&engine, "/path/to/model", cpu_options);

// Generate text
const char* prompt = "Hello, how are you?";
char* response = LlmInferenceEngine_GenerateResponse(engine, prompt);
```

---

## 🔧 Technical Details

### Build System
- **Build Tool:** Bazel 7.4.1
- **Compiler:** Platform-specific (MSVC, GCC, Clang)
- **Dependencies:** TensorFlow Lite, OpenCV, Abseil

### File Types
- **Windows:** `.dll` (shared libraries), `.lib` (import libraries)
- **Linux:** `.so` (shared libraries)
- **macOS:** `.dylib` (dynamic libraries), `.metallib` (Metal shaders)
- **Models:** `.task` (bundled model files), `.tflite` (TensorFlow Lite models)

### Performance Notes
- OpenGL ES on Windows uses ANGLE (DirectX backend) - performance may vary vs native OpenGL
- Metal on macOS provides best GPU performance on Apple Silicon
- LLM inference is CPU-only on desktop (GPU support planned by MediaPipe team)

---

## 📄 License

This project is licensed under the **MIT License**.

### Original Work
- **MediaPipe** by Google LLC
  - Repository: [google/mediapipe](https://github.com/google/mediapipe)
  - License: Apache 2.0 License

### Dependencies
- **TensorFlow Lite** by Google
  - Repository: [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow)
  - License: Apache 2.0 License

### This Distribution
- **Build scripts and optimizations** by [ocentra](https://github.com/ocentra)
  - Repository: [ocentra/mediapipe](https://github.com/ocentra/mediapipe)
  - License: MIT License

```
MIT License

Copyright (c) 2024 Google LLC (Original MediaPipe)
Copyright (c) 2024 ocentra (Build optimizations and distribution)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🤝 Contributing

This is a **binary distribution repository**. For source code contributions, please visit:
- [ocentra/mediapipe](https://github.com/ocentra/mediapipe) - Build scripts and optimizations
- [google/mediapipe](https://github.com/google/mediapipe) - Original MediaPipe implementation

---

## 📞 Support

**Issues with builds:**
- Open an issue at [ocentra/mediapipe Issues](https://github.com/ocentra/mediapipe/issues)

**MediaPipe questions:**
- See [google/mediapipe Documentation](https://ai.google.dev/edge/mediapipe/solutions/guide)

**TabAgent integration:**
- Contact: [TabAgent Server](https://github.com/ocentra/TabAgent)

---

## 🌟 Acknowledgments

- **Google LLC** - Original MediaPipe framework
- **TensorFlow team** - TensorFlow Lite inference engine
- **ANGLE Project** - OpenGL ES on Windows

---

## 📊 Status

**Current Status:**
- **Platforms:** 3 (Windows / Linux / macOS) - 🚧 Builds in progress
- **Build Variants:** TBD
- **Task Coverage:**
  - Vision: ✅ Planned
  - GenAI/LLM: ✅ Planned
  - Text: ✅ Planned
  - Audio: ✅ Planned
- **GPU Support:**
  - Windows: OpenGL ES (via ANGLE)
  - Linux: OpenGL ES
  - macOS: Metal

**Last Updated:** October 2024

---

**⚡ On-device ML at the edge. Use MediaPipe for production-ready inference!**
