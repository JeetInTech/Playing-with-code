# ✦ HANDS CONNECT ✦

### Advanced Interactive AR Visualizer Suite

_Real-time hand tracking • Audio-reactive visuals • Medical X-ray simulation • Cross-window physics_

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](#-license)
![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange)
![WebGL](https://img.shields.io/badge/WebGL-Shaders-green)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands%20%7C%20Holistic-purple)
![Web Audio](https://img.shields.io/badge/Web%20Audio-API-red)

---


## 🌟 Overview

**Hands Connect** is a collection of cutting-edge, browser-based augmented reality experiences built entirely with vanilla HTML5, JavaScript, CSS, and GPU shaders. The project pushes the boundaries of what's possible in the browser by combining Google's **MediaPipe** hand/body tracking with the **Web Audio API**, **WebGL**, and **HTML5 Canvas** to create immersive, interactive, and visually stunning experiments.

Every experience is designed for real-time rendering (often near 60fps on capable hardware), requires **zero installation**, and works directly in modern browsers (Chrome/Edge recommended).

---

## 📂 Complete File Manifest

```
hands connect/
│
├── README.md                              # This documentation file
├── LICENSE                                # MIT license
├── PROJECTS_DETAILED.md                   # Additional project notes
│
├── hands.html                             # Core AR hand tracker with audio engine
├── particle.html                          # Interactive 3D particle sphere & text morph
├── xray.html                              # X-Ray hand portal engine (v1)
├── xray-v2.html                           # X-Ray body scanner with realistic radiography (v2)
│
├── music/                                 # Audio-reactive singularity visualizers
│   ├── audio-singularity.html             # ★ Primary — GPU-powered singularity engine
│   ├── audio-singularity copy.html        # Iteration: Fire particle system prototype
│   ├── audio-singularity copy 2.html      # Iteration: Minimal 3D audio mesh
│   ├── audio-singularity copy 3.html      # Iteration: Fluid fire ribbon system
│   └── audio-singularity copy 4.html      # Iteration: WebGL shader (disabled) + 3D mesh
│
└── blackhole/                             # Cross-window spacetime physics
    ├── blackhole-twin.html                # Twin-window gravitational black hole sphere
    └── wormhole-bridge-v2.html            # Cross-tab file transfer wormhole portal
```

---

## 🔬 Module Deep Dive

### 1. 🖐️ Core Hand Tracker — `hands.html`

> **The foundation of the entire suite.**

A full-featured AR hand-tracking experience with built-in audio synthesis and multiple visual themes.

**Technical Architecture:**
- **MediaPipe Hands API** — Tracks up to 2 hands simultaneously with 21 landmark points per hand (wrist, knuckles, fingertips) at `modelComplexity: 1`
- **Web Audio API AudioEngine** — Constructs a procedural audio graph at runtime:
  - **Continuous Hum:** A `sine` wave oscillator whose frequency is modulated in real-time by the Euclidean distance between your two index fingertips. Moving hands apart sweeps the pitch from low bass to high treble
  - **Zap Effect:** A `sawtooth` waveform burst triggered on pinch gestures, creating an electrical discharge sound effect
  - Volume and frequency are interpolated smoothly to avoid audio pops
- **Visual Themes:**
  - Neon cybernetic skeleton with glowing joint nodes
  - Dynamic lightning/laser links connecting extended fingertips across both hands
  - Bass-reactive camera overlay darkening on musical beats
- **Gesture Detection:**
  - **Pinch Detection** — Computes the ratio of thumb-to-index distance against overall palm scale to detect pinch gestures reliably regardless of hand distance from camera
  - **Spread Detection** — Finger extension recognition using landmark distance ratios

**Key Constants & Parameters:**
| Parameter | Value | Purpose |
|-----------|-------|---------|
| `maxNumHands` | `2` | Simultaneous hand tracking |
| `modelComplexity` | `1` | Balanced speed/accuracy |
| `minDetectionConfidence` | `0.7` | Detection threshold |
| `minTrackingConfidence` | `0.5` | Tracking persistence |

---

### 2. 🎵 Audio-Reactive Singularity — `music/audio-singularity.html`

> **The flagship visualizer — a GPU-accelerated audio-reactive hand energy engine.**

This is the most technically complex file in the repository. It fuses real-time hand tracking with multi-source audio analysis to render a living, breathing energy singularity between your hands.

**Audio Sources (3 modes):**
| Mode | Input | How It Works |
|------|-------|-------------|
| **MIC** | Microphone | `getUserMedia({ audio })` → `MediaStreamSource` → `AnalyserNode` (no speaker output) |
| **SYSTEM AUDIO** | Desktop/Spotify audio | `getDisplayMedia({ audio, video })` → Video tracks disabled → Audio stream captured via `MediaStreamSource` |
| **AUDIO FILE** | Drag-and-drop / file picker | `<audio>` element → `MediaElementSource` → `AnalyserNode` → Speaker output |

**Frequency Analysis Pipeline:**
```
Raw FFT (1024-point) → 512 frequency bins → Split into 3 bands:
  ├── Bass:  bins[0 .. 1.5%]   — Controls pulse width, core glow, beat detection
  ├── Mids:  bins[1.5% .. 18%] — Controls wave turbulence, shell wobble
  └── Highs: bins[18% .. 100%] — Controls spark flicker, fin detail, shell ripple
```

**Beat Detection Algorithm:**
```javascript
// Triggered when bass rises sharply above threshold
if (bass > 0.08 && bassRise > 0.028) {
    beatPulse = clamp(beatPulse + 0.42 + bassRise * 4.2, 0, 1);
}
// beatPulse decays at 2.9/sec, creating burst-then-fade dynamics
```

**Gesture-Controlled Playback:**
- **Two-hand pinch** (both thumbs touching index fingers) toggles audio file play/pause
- Uses `pinchReadyAt` timing + `130ms hold` threshold to prevent accidental triggers
- Supports both **local pinch** (same-hand) and **bridge pinch** (cross-hand, thumbs/indices touching between hands)
- 900ms cooldown between toggles

**WebGL Shader Engine (`#fx` canvas):**
- Runs a fullscreen fragment shader on a secondary canvas with `mix-blend-mode: screen`
- **GPU auto-detection:** Checks `WEBGL_debug_renderer_info` — only enables on NVIDIA GPUs (RTX/GTX) at 55% resolution; falls back to CPU 2D on integrated graphics
- Shader computes per-pixel energy fields from:
  - 10 fingertip attractors (5 per hand)
  - Audio-modulated strand functions with sinusoidal wave displacement
  - Pulsing shell radius driven by bass
  - Orbiting amber spark nodes driven by highs
- Color palette: `purple → electric pink → amber fire → white hot` based on field density

**2D Canvas Visualization Layers (CPU fallback):**
1. **Ambient Fire Aura** — Radial gradient halo centered on fingertip centroid, 44 orbiting spark dots
2. **Single Waveform Ribbon** — Multi-frequency sinusoidal displacement with audio-reactive amplitude (bass × 92 + mids × 56 + beatPulse × 105 pixels)
3. **3D Audio Mesh** — 32-segment × 12-radial cylindrical mesh with depth-sorted quad rendering, `screen` blend mode
4. **Liquid Audio Shell** — 36-point audio-reactive ellipsoid with frequency-bar spikes
5. **Fingertip Singularity Core** — White-hot radial gradient (bass × 34 + mids × 14 pixel radius)

**Hand Skeleton Rendering:**
- Purple translucent bones (opacity scales with `visualEnergy`)
- Amber-glowing fingertip nodes (5.5px radius)
- Pinch indicator line between thumb and index with glow intensity inversely proportional to `pinch.ratio`

---

### 3. 🎵 Audio Singularity Iterations — `music/` copies

These files document the evolution of the audio visualizer across design iterations:

#### `audio-singularity copy.html` — *Fire Particle Prototype*
- **Orange/gold fire aesthetic** (warm color palette instead of purple)
- Spawns up to **3,000 fire particles** + **500 spark particles** from hand landmarks
- Particle behavior: upward drift (`vy -= 0.15`), bass-reactive turbulence, mid-frequency sine wobble
- **Energy tendrils:** Lightning-bolt paths between adjacent fingertips on high-frequency spikes
- **Beat detection:** Triggers spark bursts from all 5 fingertips simultaneously

#### `audio-singularity copy 2.html` — *Minimal 3D Mesh*
- Stripped-down version with only the **3D audio mesh** visualization
- Green skeleton + red joint dots (classic MediaPipe style)
- Single-hand mode: generates a phantom anchor point for the mesh endpoint
- No gesture control, no WebGL shader — pure Canvas 2D proof-of-concept

#### `audio-singularity copy 3.html` — *Fluid Fire Ribbon*
- Introduces the **`drawFluidFireRibbon()`** function — a thick, undulating ribbon connecting fingertips to the centroid
- 18-segment parametric curve with audio-frequency displacement
- **3-layer stroke rendering:** purple outer → electric pink middle → amber inner core
- Full pinch-to-play gesture system with `900ms` cooldown
- 44 orbiting ambient fire particles

#### `audio-singularity copy 4.html` — *WebGL Shader (Disabled)*
- Includes the complete WebGL shader pipeline but with `ENABLE_WEBGL_FX = false`
- Contains the full strand/shell/spark fragment shader (identical to final version)
- Uses `beatPulse` decay system and `audioPresence` smoothing
- Falls back to the 3D audio mesh on Canvas 2D

---

### 4. 🩻 X-Ray Hand Portal Engine — `xray.html`

> **A cybernetic x-ray scanner that renders your body as a holographic medical display.**

**MediaPipe Holistic Integration:**
- Uses the full **Holistic** model (not just Hands) to track:
  - 33 pose landmarks (full body skeleton)
  - 21 landmarks per hand (left + right)
  - Segmentation mask (body silhouette)
- `enableSegmentation: true` provides a per-pixel body mask for particle containment

**Portal Geometry:**
- The "X-ray window" is defined by 4 anchor points: left thumb, left index, right index, right thumb
- These 4 hand landmarks form a dynamic quadrilateral that acts as a viewport into the X-ray dimension
- All rendering is clipped to this portal region

**Neural Energy Body System:**
- **10,000 body particles** rendered in 3 depth layers (BACK: 0–3000, MID: 3000–7000, FRONT: 7000–10000)
- Particles are distributed using the **segmentation mask**: valid body pixels are sampled and mapped to portal coordinates
- **Drift compensation:** All particles shift by `bodyDelta` (frame-to-frame chest center movement) to stay attached to the moving body
- **Head density:** First 1,500 particles are specifically allocated to cover the head region with elliptical random distribution

**Visual Effects:**
- Neon cyan/teal skeleton overlay
- Glitch system: random horizontal slice displacement, white flash noise, corrupt hex text overlay
- Pulsing scan line sweeping vertically across the portal
- HUD with live joint count, random hex codes, stability percentage, and flux readings
- Joint tags with micro-telemetry labels

---

### 5. 🩻 X-Ray V2 — Realistic Radiography — `xray-v2.html`

> **A photorealistic medical radiograph simulation with anatomical bone structure rendering.**

**Key V2 Enhancements Over V1:**

**Realistic X-Ray Color Palette:**
```javascript
// Anatomical zone classification
Zone 0 (Head):    Bone — rgba(220, 235, 255)   Tissue — rgba(160, 185, 210)
Zone 1 (Torso):   Bone — rgba(200, 220, 245)   Tissue — rgba(140, 170, 200)
Zone 2 (Limbs):   Bone — rgba(180, 205, 235)   Tissue — rgba(120, 155, 190)
Zone 3 (Extrems): Bone — rgba(160, 190, 225)   Tissue — rgba(100, 140, 180)
```

**`BodyParticle` Class (Float32Array optimization):**
- Each of the 10,000 particles stores: `x, y, targetX, targetY, vx, vy, size, life, colorTier, isBone`
- **Bone vs. Tissue classification:** 30% of particles are classified as `bone` (brighter, larger, higher opacity) vs. 70% as `tissue` (dimmer, more translucent)
- Uses `Float32Array`-backed storage to minimize GC pressure at 60fps

**New Rendering Layers:**
1. **`drawTissueDensity()`** — Soft radial gradients at major body landmarks simulating tissue density (chest gets the densest overlay)
2. **`drawXrayBoneHints()`** — Anatomically accurate bone structure lines:
   - Spine: 10-segment vertebral column from mid-shoulders to mid-hips
   - Ribcage: 12 pairs of curved rib arcs emanating from the spine
   - Limb bones: Straight lines with slight organic wobble along major limb segments
3. **`drawFilmGrain()`** — 2,000 random 1-pixel white dots at low opacity simulating radiographic film noise
4. **`drawXrayVignette()`** — Soft black radial gradient at portal edges simulating X-ray light falloff

**Fluoroscopy Scan Line:**
- Slow vertical sweep with a soft blue-white gradient tail (80px trailing fade)
- Creates the classic "scanning" effect seen in real fluoroscopy equipment

---

### 6. ⚫ Twin Black Hole — `blackhole/blackhole-twin.html`

> **A cross-window gravitational physics simulation with synchronized black holes.**

**Twin-Window Architecture:**
- Uses `BroadcastChannel` API (with `localStorage` fallback) to synchronize state between multiple browser tabs
- Each window broadcasts its screen position (`window.screenX`, `window.screenY`) 60 times per second
- The black hole gravitates toward the **centroid** of all active windows

**Particle System:**
- **3,000 particles** distributed on a **Fibonacci sphere** (golden angle = `π(3 - √5)`)
- All positions stored in `Float32Array` typed arrays (`px`, `py`, `pz`, `vx`, `vy`, `vz`, `tx`, `ty`, `tz`, `twinkle`)
- Spring-damper physics: particles are attracted to rotating target positions with `0.038` spring constant and `0.9` damping
- 3D→2D perspective projection with `fov = 760` and `camZ = 900`

**Black Hole Rendering:**
- **Event horizon:** 48px solid black circle
- **Photon sphere:** Pulsing ring at 54px + sine modulation
- **Accretion disk:** 7 concentric arc segments with perspective tilt (`0.33 + sin` oscillation), 180 scattered hot particles
- **Gravitational lensing:** Stars within 220px of the black hole center are visually displaced outward, simulating spacetime curvature

**Background:**
- 500 stars with individual twinkle phases
- Dual-layer nebula radial gradients (deep blue + deep purple)

---

### 7. 🌀 Wormhole File Transfer — `blackhole/wormhole-bridge-v2.html`

> **Drag a file into a black hole, watch it traverse an Einstein-Rosen bridge, and receive it in another tab.**

**File Transfer Protocol:**
```
Sender Tab                              Receiver Tab
─────────                              ─────────────
1. File dropped → intake animation
2. ArrayBuffer sliced into 64KB chunks
3. BroadcastChannel → file-start msg   → handleFileStart()
4. BroadcastChannel → file-chunk msgs  → handleFileChunk()
5. BroadcastChannel → file-complete    → handleFileComplete()
                                        6. Blob reconstructed → preview/download
```

**Transfer Animations:**
| Phase | Duration | Visual |
|-------|----------|--------|
| **Intake** | 1.8s | File thumbnail spirals into the sender's black hole (shrinking + rotating), portal glows purple |
| **Bridge Transit** | `bridgePacketT` 0→1 | Glowing white orb travels along Bézier curve between portals with 10-particle trail |
| **Emerge** | 1.2s | 80 burst particles explode from receiver's portal, golden flash expanding outward |
| **Display** | — | Image preview card with download/open/dismiss actions |

**Spacetime Bridge Rendering:**
- **Wireframe tube:** 28 cross-section ellipses × 14 longitudinal curves along a quadratic Bézier path
- Tube radius pinches to 25% at the midpoint (Einstein-Rosen throat)
- 400 stream particles flow through the tube interior with sinusoidal wobble
- Soft inner glow gradients along the tube length

**Deep Space Background (pre-rendered):**
- Static nebula clouds rendered once to an offscreen canvas (7 major + 12 wisp nebulae)
- 800 stars with parallax gravitational lensing near both portals
- 6% of stars are "bright" with lens flare cross-spikes

---

### 8. 🔮 Interactive Particle Sphere — `particle.html`

> **10,000+ particles forming a rotating sphere that morphs to spell words.**

**Fibonacci Sphere Distribution:**
```javascript
const goldenAngle = Math.PI * (3 - Math.sqrt(5));
// For particle i of N:
y = 1 - (i / (N - 1)) * 2;           // Uniform Y distribution [-1, 1]
radius = Math.sqrt(1 - y * y);         // XZ radius at given Y
theta = goldenAngle * i;               // Golden angle spacing
x = Math.cos(theta) * radius * R;
z = Math.sin(theta) * radius * R;
```

**Text Morphing System:**
- Uses an **offscreen canvas** to render target text with `ctx.fillText()`
- Scans pixel data to find all filled pixels, creating a point cloud of target positions
- Each particle is assigned a target position in the text and smoothly interpolates toward it using spring physics
- Cycles through configurable word arrays

**Physics Engine:**
- **Cursor repulsion:** Particles within a threshold distance from the mouse are pushed away with force inversely proportional to distance squared
- **Spring return:** `vx += (targetX - px) * 0.038` with `0.9` damping factor
- **3D rotation:** Continuous Y-axis rotation applied to target positions before projection
- **Depth-of-field:** Particle size and opacity scale with perspective projection factor `fov / (z + camZ)`

---

## 🛠️ How to Run

These experiments use secure browser APIs (Camera, `BroadcastChannel`, WebGL, `getDisplayMedia`) that require a proper HTTP server.

### Quick Start (choose one):

```bash
# Option 1: Python (built-in)
python -m http.server 5500

# Option 2: Node.js (npx)
npx serve .

# Option 3: VS Code
# Install "Live Server" extension → Right-click any .html → "Open with Live Server"
```

Then navigate to `http://localhost:5500/` and open `hands.html` (or whichever file you want).

### System Requirements

| Requirement | Minimum | Recommended |
|------------|---------|-------------|
| Browser | Chrome 90+ / Edge 90+ | Chrome 120+ |
| Camera | Any webcam | 1080p webcam |
| GPU | Integrated graphics | NVIDIA GTX/RTX (for WebGL shaders) |
| Audio | Built-in mic | External mic / Spotify / Audio file |

### Tips for System Audio Capture (Windows)

When using the **SYSTEM AUDIO** button in the audio-singularity visualizer:
1. Click **"SYSTEM AUDIO"** in the top panel
2. In the Chrome/Edge share dialog, select **"Entire Screen"**
3. **Check the "Share system audio" toggle** at the bottom-left of the dialog
4. Click **"Share"**

> ⚠️ Window-level sharing (selecting a specific app window) usually does **not** capture audio. You must share the entire screen or a browser tab.

---

## 🏗️ Technical Architecture

### Core Dependencies (all via CDN, zero build step)

| Library | Version | Purpose |
|---------|---------|---------|
| `@mediapipe/hands` | Latest | 21-point hand landmark detection |
| `@mediapipe/holistic` | Latest | Full body pose + hand + segmentation |
| `@mediapipe/camera_utils` | Latest | Webcam frame loop management |
| `@mediapipe/drawing_utils` | Latest | Landmark visualization helpers |
| `@mediapipe/control_utils` | Latest | UI control panel utilities |

### Performance Optimizations

- **`Float32Array` particle storage** — Avoids object allocation and GC pauses for 10,000+ particle systems (`xray-v2.html`, `blackhole-twin.html`)
- **`Path2D` body clipping** — Single `ctx.clip()` call for the entire body polygon instead of per-particle bounds checking (`xray-v2.html`)
- **Pre-rendered backgrounds** — Static nebula/star fields rendered once to offscreen canvases (`wormhole-bridge-v2.html`)
- **GPU auto-detection** — WebGL shader scales or disables based on detected GPU vendor (`audio-singularity.html`)
- **`requestAnimationFrame` loop** — All animations use `rAF` with delta-time capping at 33ms to prevent physics explosions on tab-switch resume
- **Lazy AudioContext** — Audio contexts only created on first user interaction to comply with browser autoplay policies

---

## 🧠 Core Concepts & Ideas

### 1. Augmented Reality Without Hardware
The entire project demonstrates that sophisticated AR experiences are possible with nothing but a browser and a webcam. No VR headset, no depth sensor, no native app — just JavaScript and a camera feed.

### 2. Audio as a Visual Medium
Sound is decomposed into frequency bands (bass, mids, highs) and mapped to visual parameters (size, color, speed, opacity, displacement). This creates a synesthetic experience where you can literally *see* the music.

### 3. The Body as an Interface
Instead of clicking buttons, you use your hands as controllers. Pinch to play/pause music. Spread your fingers to create energy connections. Move your hands apart to modulate a synthesizer's pitch. The interface disappears — your body *is* the interface.

### 4. Cross-Window Communication
The blackhole/wormhole experiments treat separate browser windows as portals in a shared spacetime. This is achieved through the `BroadcastChannel` API, turning the mundane act of "file transfer" into a visually spectacular traversal of an Einstein-Rosen bridge.

### 5. Medical Imaging Simulation
The X-ray engine goes beyond simple skeleton overlay — it simulates tissue density gradients, bone classification (cancellous vs. cortical), film grain, fluoroscopic scan lines, and diagnostic HUD overlays, approaching the visual fidelity of real DICOM radiograph viewers.

---

## 📜 License

```
MIT License

Copyright (c) 2026 Hands Connect Contributors

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

**In plain English:** You are free to use, copy, modify, and distribute this project (including commercially). The main requirement is to keep the MIT license notice in copies/substantial portions of the software. Enjoy! 🚀

---


_Built with 🖐️ hands, 🎵 sound, and ☕ caffeine._
