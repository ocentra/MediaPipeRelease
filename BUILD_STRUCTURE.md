# MediaPipe Release Directory Structure

**Purpose:** Pre-compiled MediaPipe C++ Task APIs for all platforms  
**Strategy:** "Squeeze Everything Out" - Build ALL tasks, use what you need later!

---

## 📂 Directory Structure

```
MediaPipeRelease/
├── cpu/
│   ├── windows/standard/         # Windows CPU (XNNPACK delegate)
│   ├── linux/standard/           # Linux CPU (XNNPACK delegate)
│   └── macos/
│       ├── arm/                  # Apple Silicon (M1/M2/M3/M4)
│       └── intel/                # Intel Macs
│
└── gpu/
    ├── windows/opengl-es/        # Windows GPU (OpenGL ES via ANGLE)
    ├── linux/opengl-es/          # Linux GPU (OpenGL ES native)
    └── macos/metal/              # macOS GPU (Metal acceleration)
```

---

## 📦 What Goes in Each Directory

### **CPU Builds** (6 total: 3 Windows + 2 Linux + 2 macOS)

Each directory will contain:

#### **Vision Task Libraries** (~15 DLLs/SOs/Dylibs)
- `mediapipe_object_detector.*`
- `mediapipe_image_classifier.*`
- `mediapipe_face_detector.*`
- `mediapipe_face_landmarker.*`
- `mediapipe_face_stylizer.*`
- `mediapipe_pose_landmarker.*`
- `mediapipe_hand_landmarker.*`
- `mediapipe_holistic_landmarker.*`
- `mediapipe_gesture_recognizer.*`
- `mediapipe_image_segmenter.*`
- `mediapipe_interactive_segmenter.*`
- `mediapipe_image_embedder.*`
- `mediapipe_image_generator.*`
- `mediapipe_pose_detector.*`
- `mediapipe_hand_detector.*`

#### **GenAI/LLM Libraries** (~1 DLL/SO/Dylib)
- `libllm_inference_engine_cpu.*`

#### **Text Task Libraries** (~3 DLLs/SOs/Dylibs)
- `mediapipe_language_detector.*`
- `mediapipe_text_classifier.*`
- `mediapipe_text_embedder.*`

#### **Audio Task Libraries** (~2 DLLs/SOs/Dylibs)
- `mediapipe_audio_classifier.*`
- `mediapipe_audio_embedder.*`

#### **Core & Components** (Foundation libraries)
- `mediapipe_core.*`
- `mediapipe_components.*`
- Additional utility libraries

#### **Dependencies** (Runtime libraries)
- TensorFlow Lite runtime
- OpenCV libraries (for Vision)
- Abseil C++ libraries
- SentencePiece (for GenAI/LLM)
- Platform-specific runtime DLLs

**Total per CPU build:** ~25-30 libraries + dependencies

---

### **GPU Builds** (3 total: Windows + Linux + macOS)

Same structure as CPU builds, but with GPU delegate enabled:
- **Windows:** OpenGL ES via ANGLE (DirectX backend)
- **Linux:** OpenGL ES native
- **macOS:** Metal acceleration

**Note:** GenAI/LLM is CPU-only on desktop (no GPU support yet)

---

## 🎯 Build Variants Summary

| Platform | CPU Delegate | GPU Delegate | Total Builds |
|----------|--------------|--------------|--------------|
| **Windows** | XNNPACK | OpenGL ES (ANGLE) | 2 |
| **Linux** | XNNPACK | OpenGL ES | 2 |
| **macOS** | XNNPACK | Metal | 2 |
| **TOTAL** | 3 | 3 | **6** |

---

## 🚀 Usage Pattern

### **Load What You Need:**
```rust
// Rust FFI example
let vision_lib = Library::new("MediaPipeRelease/cpu/windows/standard/mediapipe_object_detector.dll")?;
let llm_lib = Library::new("MediaPipeRelease/cpu/windows/standard/libllm_inference_engine_cpu.dll")?;
```

### **Switch CPU/GPU:**
```rust
// CPU build
let detector = Library::new("MediaPipeRelease/cpu/windows/standard/mediapipe_face_detector.dll")?;

// GPU build (same API, GPU-accelerated)
let detector = Library::new("MediaPipeRelease/gpu/windows/opengl-es/mediapipe_face_detector.dll")?;
```

---

## 📊 Size Estimates

**Per Build Directory:**
- Vision libraries: ~50-100 MB
- GenAI/LLM: ~20-50 MB
- Text libraries: ~10-20 MB
- Audio libraries: ~5-10 MB
- Dependencies: ~50-100 MB
- **Total per build:** ~150-300 MB

**Total Repository Size:**
- 6 builds × 150-300 MB = **~1-2 GB** (with Git LFS)

---

## 🔧 Build Order

Builds will be created in this order:

1. **Windows CPU** (standard)
2. **Windows GPU** (opengl-es)
3. **Linux CPU** (standard)
4. **Linux GPU** (opengl-es)
5. **macOS CPU** (arm + intel)
6. **macOS GPU** (metal)

Each build extracts **ALL** MediaPipe tasks - "not a single drop left!" 🍊

---

## ✅ Current Status

- [x] Directory structure created
- [x] `.gitkeep` files added
- [ ] Windows builds (pending)
- [ ] Linux builds (pending)
- [ ] macOS builds (pending)

---

**Next:** Install Bazel and start building! 🚀

